---
layout: default
title: 3) ML 5종 추론 모델
permalink: /docs/data-method/ml.html
parent: 2. 데이터와 방법론
nav_order: 3
---

# 3) ML 5종 추론 모델

> 💡 **쉽게 말하면**: 지금 점수(TVI)가 현재의 건강검진이라면, 여기서는 "이 마을이 앞으로 어떻게 될까"를 예측합니다. 소멸 위험을 분류하고(XGBoost), 인구가 몇 년 뒤 어디까지 줄지 그리며(Prophet), 비슷한 처지의 마을끼리 묶어(K-Means) 무엇이 통했는지 벤치마킹합니다.

> `backfill_tvi_feature_snapshot.py`로 36개월 실데이터에서 **5,508행**(153마을 × 36개월, 피처 22개) 훈련 샘플 적재 후 훈련 완료.

| 문제 | 모델 | 산출 결과 |
|---|---|---|
| 소멸 위험 분류 | XGBoost Classifier (`scale_pos_weight` 불균형 보정) | `risk_level`(danger/warning/safe) + 신뢰도 % |
| 12개월 후 TVI 회귀 | XGBoost Regressor + Ridge | `tvi_next_12m` + 80% 신뢰 구간 |
| 인구 시계열 예측 | Prophet (`yearly_seasonality`) | 미래 인구 + 80% 신뢰 구간 밴드 |
| 마을 유형 군집화 | K-Means (K=6) | `cluster_label`("고령화+교통고립형" 등) + 벤치마킹 |
| What-if 처방 계수 | Ridge Regression | 수작업 상수 → 데이터 기반 처방 효과 계수 교체 |

**ML 추론 API** — `GET /api/townpulse/villages/{villageCode}/ml-prediction`

```json
{
  "risk_level_predicted": "danger",
  "risk_confidence": 0.87,
  "tvi_next_12m": 34.2,
  "tvi_lower_bound": 29.1,
  "tvi_upper_bound": 39.4,
  "cluster_label": "고령화+교통고립형",
  "similar_villages": [{ "village_name": "...", "tvi_score": 0, "top_prescription": "..." }]
}
```

> 피처 22개 = 인구 5 · 빈집 1 · 교통 4 · 인프라 4 · 행정 1 · TVI 7.
