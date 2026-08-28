---
layout: default
title: 4) ERD & 데이터 흐름
permalink: /docs/system/erd.html
parent: 3. 시스템 구조
nav_order: 4
---

# 4) ERD & 데이터 흐름

> 💡 **쉽게 말하면**: 데이터를 어떤 표(테이블)에 어떻게 나눠 담는지, 그리고 매달 어떤 순서로 자동 수집해 점수를 다시 계산하는지를 정리했습니다. 하나의 큰 스냅샷 대신 인구·건물·에너지·교통·인프라·통계를 각각의 표로 쪼개 관리합니다.

## 10-1. MVP ERD — 25개 물리 테이블 (심사 기준)

| 그룹 | 테이블 |
|---|---|
| 공간/마을 | `region`, `village`, `infrastructure_facility` |
| API 스냅샷 | `snap_population`, `snap_building`, `snap_energy`, `snap_transport`, `snap_infrastructure`, `snap_statistics` |
| TVI 산출 | `tvi_score`, `tvi_norm_state` |
| 처방 라이브러리 | `prescription_type`, `prescription_indicator`, `prescription_fund_source`, `dispatch_rule`, `budget_unit_price` |
| 처방 결과 | `prescription_result`, `budget_estimate` |
| ML 원천 | `tvi_feature_snapshot`, `ml_inference_result` |
| SaaS 운영 | `organization`, `subscription`, `townpulse_user`, `report` |
| 운영 보조 | `public_data_sync_job` |

> 정규화: `DATA_SNAPSHOT` 1개 → `SNAP_*` 6개 분리(SRP) · `DISPATCH_RULE` 신설(OCP) · `fiscal_self_reliance` → `REGION`(시군구 중복 방지) · `tvi_feature_snapshot`/`ml_inference_result` MVP 포함(ML 원천·캐시).
> **역정규화 유지:** `risk_level`·`tvi_delta`(히트맵 성능·알림) · `ai_description`(Exaone 재호출 비용 절감).

## 10-2. 배치 수집 흐름

```
[APScheduler — 매월 1일 03:00 KST] PublicDataSyncOrchestrator.collect_all()
  ├─ village.update_geocode_from_vworld()   (vworld → VILLAGE lat/lng 선행)
  ├─ snap_population.ingest_core()          (행안부 #2 #3)
  ├─ snap_building.ingest()                 (건축HUB #1)
  ├─ snap_transport.ingest_for_village()    (TAGO 2단계 + 정류소 + ODsay 폴백)
  ├─ snap_statistics.ingest()               (KOSIS 시군구 캐시 → 153마을 배분)
  └─ tvi_score.recalculate_all()            (SNAP 5종 교차 → TVI v2.1)
[3일 분할] collect_migration_chunk(0→1→2)   (인구이동 #4, 쿼타 10,000/일 한도)
[연간 1월] ingest_fiscal_all()              (지방재정365 → REGION.fiscal_self_reliance)
```

## 10-3. TVI 산출 데이터 흐름

```
SNAP_POPULATION·BUILDING·ENERGY·TRANSPORT·STATISTICS
  → recalculate_all()
     1. 하이브리드 빈집율 (확정×1.0 + 유력×0.75 + 추정×0.40×M_trade) / 전체주거건물수
     2. 4대 부문 점수 (pop_decline · vacancy · bus_interval · infra, 각 0~100)
     3. 5대 시나리오 판정 → 동적 가중치 선택
     4. TVI = Σ(부문점수 × 시나리오가중치)
  → TVI_SCORE (tvi_score, risk_level, selected_scenario, tvi_delta)
```

## 10-4. 배포 구성

```
Vercel (town.www · Next.js 14)  ──REST/SSE──►  Fly.io (com.pulse · FastAPI + Uvicorn)
                                                 core/matrix/ (Keymaker·Oracle·Trinity·Smith)
                                                 ├─ NeonDB (25 테이블 · asyncpg · sslmode=require)
                                                 ├─ Exaone 3.5:7.8b (Ollama 로컬 · 소버린 AI)
                                                 └─ 공공 API 19종 + ODsay 폴백
```
