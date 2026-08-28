---
layout: default
title: 3) 프론트엔드 & 사용자 워크플로우
permalink: /docs/system/frontend.html
parent: 3. 시스템 구조
nav_order: 3
---

# 3) 프론트엔드 & 사용자 워크플로우

> 💡 **쉽게 말하면**: 공무원이 지도에서 마을 하나를 클릭하면, 옆에 그 마을의 생존 점수와 취약 지표가 뜨고, AI에게 물어보면 처방과 예산을 설명해 줍니다. 마지막엔 대화 내용을 그대로 담은 A4 보고서를 PDF로 뽑습니다.

## 9-1. 기술 스택 & 화면

Next.js 14 (App Router) · TypeScript · Tailwind(`darkMode: class`) · next-themes · **Leaflet**(Choropleth 히트맵) · **Recharts**(레이더·바·게이지) · **Zustand**(인증·선택 EMD·채팅) · html2canvas + jspdf(PDF).

| 경로 | 페이지 | 설명 |
|---|---|---|
| `/` | 랜딩 + 대시보드 요약 | 인트로 비디오 → Hero → 기능 → 통계 → CTA |
| `/login` | 로그인 | 이메일/비밀번호 + 데모 자동 토큰 |
| `/map` | 지도 + AI 챗봇 | GIS 히트맵 + 마을 선택 + 처방 시뮬레이션 + AI 상담 |
| `/map/[villageCode]` | 마을 직접 진입 | URL로 특정 마을 코드 접근 |
| `/map/detail` | 마을 상세 | 선택 마을 전체 지표 |
| `/mypage` | 마이페이지 | AI 대화 요약 보고서 이력 + 모달 뷰어 + PDF |

<figure class="shot">
  <img src="{{ '/assets/img/map-village-select.gif' | relative_url }}" alt="마을 선택 데모 — 지도 클릭 후 TVI·QC·AI 컨설팅이 이어지는 흐름">
  <figcaption>핵심 워크스페이스 `/map` — 마을 선택부터 AI 소멸진단 컨설팅까지</figcaption>
</figure>

## 9-2. 사용자 여정

```
[랜딩 /] → 로그인 → [지도 /map]  ← 핵심 워크스페이스
  ├─ 마을 클릭 → 사이드바 요약 카드 (TVI 점수·4대 지표·QC·처방 가이드)
  ├─ AI 채팅 → 처방 생성(dispatch_rule) → SSE 스트리밍 설명
  ├─ 보고서 저장 → 대화 이력 포함 저장
  └─ 마이페이지 → 과거 보고서·채팅 복원 → PDF 출력
```

<div class="shot-grid">
  <figure class="shot">
    <img src="{{ '/assets/img/map-overview.jpg' | relative_url }}" alt="소멸위험 지도 — 충북 153개 읍·면·동 단계구분도">
    <figcaption>소멸위험 지도 — TVI 등급별 색상 음영(153개 폴리곤)</figcaption>
  </figure>
  <figure class="shot">
    <img src="{{ '/assets/img/map-village-selected.jpg' | relative_url }}" alt="마을 선택 후 사이드바 — TVI 요약과 QC 진단 활성화">
    <figcaption>마을 선택 후 — TVI 요약·QC 진단·AI 컨설팅 사이드바</figcaption>
  </figure>
  <figure class="shot">
    <img src="{{ '/assets/img/mypage-reports.jpg' | relative_url }}" alt="마이페이지 — AI 대화 요약 보고서 이력과 PDF 출력">
    <figcaption>마이페이지 — AI 대화 요약 보고서 이력·A4 PDF 출력</figcaption>
  </figure>
  <figure class="shot">
    <img src="{{ '/assets/img/home-features.jpg' | relative_url }}" alt="홈 기능 소개 섹션">
    <figcaption>홈 — 핵심 기능 소개 섹션</figcaption>
  </figure>
</div>

**마을 선택 후 사이드바 로딩 순서**
1. `GET /dashboard/map/villages/{code}` → 팝업 요약(TVI 점수·등급)
2. `GET /village-detail/{code}` → 4대 지표 (asyncio.gather 6개 테이블 병렬)
3. `GET /quality-control/villages/{code}` → 파레토·이시카와
4. `GET /prescription-results/by-village/{id}` → 기존 처방

**AI 처방 + SSE**
1. `POST /prescription-results { village_id }` → dispatch_rule 매칭 → 처방 3건 저장
2. `GET /prescription-results/{id}/stream?token=JWT` → `_build_context_prompt` → Ollama/Exaone SSE 청크 스트리밍 → `ai_description` DB 저장

## 9-3. 핵심 시각화

- **인터랙티브 GIS 맵** — 153개 EMD GeoJSON을 TVI 등급별 색상 음영. `시군구명_읍면동명` 텍스트 교차 매핑으로 **19개 → 153개 폴리곤 완전 복원**. 마커: danger=빨강 / warning=노랑 / safe=초록.
- **예산 모의 투입 시뮬레이터** — 만원 단위 슬라이더 + 9대 처방 선택 → TVI Gain 실시간, Exaone SSE 타이핑 스트리밍. 추천 질문 바(의료/교통/주거 키워드 파싱 → 동적 변경).
- **IndicatorCards** — 하이브리드 빈집 3단계(확정·유력·추정) 스택 게이지, DRT 배지, 5대 시나리오 컬러 배지, ML 예측 배지(XGBoost 신뢰도 · Prophet 밴드 · K-Means 군집).
- **AI 대화 요약 보고서** — A4 모달 뷰어, 백엔드 Exaone 동적 생성 제목, html2canvas + jspdf 고해상도 PDF, 처방 단가 `reference_source` 각주 + 재정자립도 근거 강제 출력.
