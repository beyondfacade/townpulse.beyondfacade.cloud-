---
title: 8. AI·백엔드 기술 구조
layout: default
nav_order: 8
---

# AI·백엔드 기술 구조
{: .no_toc }

## 8-1. 시스템 아키텍처

| 레이어 | 구성요소 | 기술스택 |
|---|---|---|
| 데이터 수집 | 19종 API · `{table}_repository.ingest_*` · `PUBLIC_DATA_SYNC_ORCHESTRATOR` | Python, FastAPI, asyncio.gather + Semaphore (건축HUB 20 / 에너지 10) |
| ML 분석 | XGBoost · Prophet · K-Means · Ridge | scikit-learn, XGBoost, Prophet |
| 처방 라이브러리 | 9종 처방 × 실단가 × 조건 매핑 + SimulationValidator | NeonDB (PostgreSQL serverless) |
| 소버린 AI | **Exaone 3.5:7.8b — Fly.io 서버 내 Ollama 완전 로컬** | Ollama, KiwiTokenizer, SSE |
| 예산 계산기 | 마을 데이터 × 단가 + 국비/지방비 자동 매핑 | Python |
| 시각화 | GIS 맵 + AI 챗봇 병합 대시보드 | Next.js 14, Leaflet, Recharts, Zustand, next-themes |
| 인프라 | 클라우드 배포 | Vercel · Fly.io · NeonDB |

## 8-2. 소버린 AI 처방 엔진

```
[마을 TVI 데이터]
  → [DISPATCH_RULE 처방 매핑] ← 처방 라이브러리 (DB)
       조건 탐지 → 9종 처방 선택 → 실단가 대입 → PRESCRIPTION_RESULT 생성
  → [Exaone 3.5:7.8b — Ollama 로컬] (민감 데이터 외부 전송 없음)
       입력: 마을 스냅샷 + 처방 유형 + 단가 범위 + 행정 페르소나
       KiwiTokenizer: 한국어 형태소 3,000토큰 한도 제어
       SimulationValidator: 국비/지방비 비율 수학적 검증·보정
       출력: 처방 컨설팅 텍스트 — SSE 스트리밍
  → [최종] 처방 + AI 설명 + 예산 범위 + TVI 기대치 + 국비/지방비 + reference_source 각주
```

> **핵심 원칙:** Exaone은 **처방 설명 텍스트만** 생성. 예산·TVI 기대치는 단가 라이브러리 + 마을 데이터 연산으로 산출 — "AI가 숫자를 임의로 만들지 않는다"를 구조적으로 보장.

**핵심 설계 결정**

| 항목 | 결정 | 이유 |
|---|---|---|
| AI 페르소나 위치 | `core/matrix/grid_keymaker_secret_manager.py` 상수 | `domain/value_objects/`에 두면 외부 의존성 침투 |
| SSE 토큰 전달 | URL 쿼리 `?token=JWT` | SSE는 Authorization 헤더 미지원 |
| 프롬프트 조립 | 서버 `_build_context_prompt(id)` | 클라이언트 조작 시 환각 위험 |
| 채팅 이력 저장 | `report.chat_history TEXT(JSON)` | 별도 테이블 없이 보고서에 포함 |
| 지도 필터 | `~name.like("%리")` 안전 가드 | "리" 제외, 읍면동 153개만 |
| 처방 트리거 | `dispatch_rule` 테이블 자동 매칭 | 코드 배포 없이 DB 룰 수정 |
| 교통 이중화 | TAGO → ODsay 폴백 | 증평군 등 미지원 시군구 완전 커버 |

## 8-3. 헥사고날 레이어 & 34-도메인 프랙탈

```
HTTP Request
  → Inbound Adapter: Router → Schema 검증 → inbound Mapper → DTO
  → Application Layer: UseCase Port → Interactor  (순수 Python — FastAPI·SQLAlchemy·httpx 금지)
  → Outbound Adapter: Output Port → Repository → ORM Mapper → ORM
  → NeonDB / Exaone(Ollama) / 공공 API
```

- **34개 도메인 × 11파일 프랙탈** = 물리 테이블 매핑 25개 + 오케스트레이터·보조 9개.
- 각 도메인 11파일: `router · schema · mapper · use_case · interactor · port · dto · orm · orm_mapper · repository · entity`.
- **경계 톨게이트:** mapper는 Router·Repository 경계에서만 타입 변환. UseCase~Repository 구간에 Schema·ORM 진입 금지.
- **`core/matrix/` 공통 인프라 단일 진입점:** Keymaker(비밀값) · Oracle(DB세션) · Trinity(JWT) · Neo(Declarative Base) · Morpheus · Smith · KiwiTokenizer.
- **AsyncSession 동시성 안전:** `asyncio.gather` 내 SELECT는 `no_autoflush` 블록, 레포지토리 `flush()` 직접 호출 금지.
- **확장성:** `IRegionDataSource` 포트로 `ChungbukRegionAdapter` → `NationalRegionAdapter` 교체만으로 전국 확장.
