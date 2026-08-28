---
title: 14. 핵심 기술 내역 요약
layout: default
nav_order: 14
---

# 핵심 기술 내역 요약
{: .no_toc }

| 영역 | 핵심 기술 |
|---|---|
| **지표·공간 분석** | 동적 가중치 시나리오 엔진 · 하이브리드 빈집율 알고리즘 · Haversine 공간 거리 · 파레토·피시본 QC |
| **AI·생성** | 소버린 LLM(Exaone 3.5:7.8b Ollama) · SSE 스트리밍 · System Instruction 컨텍스트 주입 · KiwiTokenizer 형태소 토큰 제어 · What-if 시뮬레이션 |
| **ML·예측** | XGBoost(분류·회귀) · Prophet(80% 신뢰 구간) · K-Means(K=6) · Ridge(처방 계수) |
| **데이터·인프라** | 19종 공공 API 연동 · APScheduler 배치 · asyncio.gather + Semaphore · 시군구 단위 캐싱 · Monthly Snapshot Skip · TAGO·ODsay 이중화 · 서버리스 PostgreSQL |

**대표 기술 상세**

- **동적 가중치 시나리오 엔진** — 고령화율·빈집율·교통 격리 지표를 진입 조건으로 검사해 5개 국면 중 하나를 자동 선택, 4대 부문 가중치를 국면별 재배분.
- **교차 검증 빈집 모델** — 행정 주택-세대 차이(확정 ×1.0) · 전력/가스 극소(유력 ×0.75) · 거래 공백 보정(추정 ×0.40×M_trade) 3단계 신뢰도 가중 합산.
- **컨텍스트 주입 프롬프트 엔지니어링** — TVI 점수·취약 지표·예산 추정치·우선순위 코드를 서버에서 System Instruction으로 조립 → LLM이 환각 없이 수치 근거 기반 답변 생성.
- **가상 스냅샷 시뮬레이션** — 정책 예산 입력을 물리 속성(배차 단축·정류장 추가)으로 변환한 임시 `VillageSnapBundle`을 메모리 생성 후 TVI 재연산으로 Gain 즉시 도출.
- **파레토 분석** — 모든 처방을 TVI Gain 기여도로 정렬, 누적 85% 달성 최소 과제 집합을 `is_core_priority`로 선별.

---

<small>
© 2026 Pulse Lab · TownPulse — 충청북도 마을생존 AI 의사결정 플랫폼 ·
2026 충청북도 공공데이터·AI 활용 창업경진대회 (제품 및 서비스 개발)
</small>
