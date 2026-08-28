---
title: 7. 지능형 의사결정 파이프라인
layout: default
nav_order: 7
---

# 지능형 의사결정 파이프라인
{: .no_toc }

수치 계산 엔진(ML·정량 통계)과 생성형 언어 모델(LLM·정성 분석)이 분리되지 않고 하나의 유기적 파이프라인으로 결합됩니다.

```mermaid
graph TD
    subgraph 1. Data Ingestion & Calculation
        A[공공 API 수집<br/>행안부·건축HUB·에너지·TAGO+ODsay·KOSIS] --> B[(DB Raw Snapshots<br/>153읍면동 × 36개월 = 5508행)]
        B --> C[TVI v2.1 통계 엔진 연산]
    end
    subgraph 2. ML 예측 추론
        C --> M1[XGBoost — 소멸위험 분류<br/>risk_level + confidence]
        C --> M2[Prophet — 인구 시계열<br/>80% 신뢰 구간 밴드]
        C --> M3[K-Means — 마을 유형 군집<br/>cluster_label K=6]
    end
    subgraph 3. What-if Simulation
        C --> D[마을 지표 모니터링]
        D --> E[정책 처방 시뮬레이터]
        E -->|물리 속성 변환 & TVI 재연산| F[TVI Gain 도출]
    end
    subgraph 4. QC & Prioritization
        F --> G[Pareto 효율 순위]
        F --> H[Fishbone 원인 감지]
        F --> I[Radar 정규화]
    end
    subgraph 5. AI Consulting & Reporting
        G & H & I & M1 & M2 & M3 --> J[Exaone 3.5:7.8b System Instruction 주입<br/>소버린 AI — Fly.io Ollama 로컬]
        J --> K[SSE 스트리밍 컨설팅<br/>5개년 로드맵 보고서]
    end
```

1. **정량 TVI 산출** — 원천 DB 기반 현재 건강 상태(TVI + 4대 지표) 즉시 평가·시각화.
2. **What-if 개입** — 공무원이 예산을 바꾸면(예: "DRT 1억 증액") 백엔드가 가상 스냅샷(`VillageSnapBundle`)을 임시 생성, 수식 엔진 재구동으로 **TVI Gain 실시간 계산**.
3. **QC 우선순위** — **파레토**로 누적 기여도 85% 달성 최우선 과제(`is_core_priority`) 선발, **피시본**으로 부문별 하락 요인 진단, **레이더**로 시계열 추이 정규화.
4. **AI 컨설턴트** — 답변 직전 TVI Gain 범위·국지비 분담·우선순위 코드·약화 항목을 `system_instruction`으로 주입 → Exaone이 환각 없이 5개년 로드맵 보고서(JSON) 빌드.
