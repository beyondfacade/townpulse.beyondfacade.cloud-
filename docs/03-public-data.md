---
title: 3. 공공데이터 활용 — 19종 API
layout: default
nav_order: 3
---

# 공공데이터 활용 — 19종 API
{: .no_toc }

> 개발계정 승인 2026-06-19 ~ 2028-06-19. 실호출 probe JSON 샘플 보관 완료. 읍면동 단위(방안 B) — "리" 배제, 153개 읍·면·동만 수집.

| # | 분류 | 데이터명 | 제공기관 | 활용 지표 |
|---|---|---|---|---|
| 1 | 인구 | 법정동별 주민등록 인구·세대현황 | 행정안전부 | `population_total`, `registered_households` |
| 2 | 인구 | 법정동별 성/연령별 인구수 | 행정안전부 | `population_65plus`, `population_youth` |
| 3 | 인구 | 지역별 인구이동 현황 | 행정안전부 | `net_youth_migration` (파생 집계) |
| 4 | 건물 | 건축HUB 건축물대장정보 | 국토교통부 | `residential_buildings` (7종 화이트리스트) |
| 5 | 에너지 | 건축물에너지정보 (전력) | 국토교통부 | `vacant_house_count` (10kWh 이하 판정) |
| 6 | 에너지 | 건축물에너지정보 (가스) | 국토교통부 | 빈집 판정 보완 |
| 7 | 교통 | 버스노선 (BusRouteInfoInqireService) | 국토교통부 | `bus_route_count`, `avg_bus_interval_min` |
| 7b | 교통 | 버스정류소 (BusSttnInfoInqireService) | 국토교통부 | `nearest_stop_distance_m`, `bus_stops_within_1km` |
| 8 | 교통 | ODsay 대중교통 API | (주)아로정보기술 | TAGO 미지원 증평군 폴백 — 11개 시군 완전 커버 |
| 9 | 공간 | VWorld 오픈API / WFS | 국토교통부 | 마을 중심좌표·재해위험지구·노인시설 |
| 10 | 통계 | KOSIS 국가통계포털 | 통계청 | `kosis_vacancy_rate`, `kosis_employment_rate`, `pop_density` |
| 11 | 재정 | 지방재정자립도 | 행정안전부 | `fiscal_self_reliance` → 국비/지방비 매핑 |
| 12 | 실거래가 | 국토부 매매 (아파트·연립·단독·오피스텔) | 국토교통부 | M_trade 보정 계수 (`recent_transactions_6m`) |
| 13 | 유통 | 소상공인공단 상가(상권)정보 | 소상공인시장진흥공단 | `food_desert_index` (식품사막) |
| 14 | 의료 | 응급의료기관 정보 | 국립중앙의료원 | 응급실 골든타임 도달률 |
| 15 | 의료 | 전국 병·의원 찾기 | 국립중앙의료원 | `medical_distance_m` |
| 16 | 의료 | 전국 약국 찾기 | 국립중앙의료원 | `medical_distance_m` 보완 |
| 17 | 교육 | KERIS 학교기본정보 | 한국교육학술정보원 | `school_distance_m` |
| 18 | 인터넷 | 행안부 무료와이파이정보 | 행정안전부 | 디지털 정주환경 참고 지표 |

## 3-1. SNAP 테이블별 JSON 키 ↔ DB 컬럼 매핑 (검증 완료)

**`snap_population`** (행안부 3종)

| DB 컬럼 | JSON 키 | 비고 |
|---|---|---|
| `population_total` | `totNmprCnt` | 총 주민등록 인구 |
| `registered_households` | `hhCnt` | 등록 세대 수 |
| `population_65plus` | `male/feml 70·80·90·100AgeNmprCnt` 합산 | 70세 이상 |
| `population_youth` | `male/feml 20·30AgeNmprCnt` 합산 | 20~39세 |
| `net_youth_migration` | 전입(`mvinAdmmCd`) − 전출(`mvtAdmmCd`) 파생 | 17개 시/도 × 2방향 스윕 |
| 조인키 | `stdgCd` (10자리 법정동코드) | `REGION.legal_dong_code` 매핑 |

> 읍면동 집계 규칙: 법정동코드 앞 8자리 접두사 기준으로 하부 "리" 인구를 **합산(Sum)** 후 읍·면·동 스냅샷 반영.

**`snap_building`** (건축HUB `getBrTitleInfo`)
- `residential_buildings` = `mainPurpsCdNm` **화이트리스트 7종 COUNT** (단독·다중·다가구·공동주택·아파트·연립·다세대). 공관·기숙사·오피스텔 의도적 제외.
- 요청 파라미터: `sigunguCd`(5자리) + `bjdongCd`(`legal_dong_code[5:10]`).

**`snap_energy`** (`getBeElctyUsgInfo`)
- `vacant_house_count` = 월 전력량 `useQty` ≤ 10kWh 건물 수, `scanned_buildings_count` = 순회 건물 수(읍면동당 최대 30개 샘플).
- 건물별 1회 호출 필수 → `asyncio.Semaphore(10)` 병렬 제어.

**`snap_transport`** (TAGO 2종 + ODsay 폴백)
- `bus_route_count`(`getRouteNoList`), `avg_bus_interval_min`(`getRouteInfoIem` `intervaltime`), `nearest_stop_distance_m`(`getCrdntPrxmtSttnList` GPS → Haversine), `bus_stops_within_1km`(노선 카탈로그 + 반경 필터), `has_drt`(노선명 키워드).

**`snap_infrastructure`** (소상공인공단·의료원 3종·KERIS·VWorld WFS)
- `food_desert_index`, `medical_distance_m`, `school_distance_m`, `senior_facility_ratio`, `disaster_risk_ratio`, `fire_station_golden_time`.
- 아동 비율 < 5% 마을은 `school_distance_m(25%)` → `senior_facility_ratio(25%)` 자동 대체. 인프라 마스터는 1회 수집 후 유지.

**`snap_statistics`** (KOSIS)
- `kosis_vacancy_rate`(DT_1JU1512), `pop_density`, `kosis_employment_rate`, `kosis_employed_growth_rate`. 5자리 시군구코드 1회 호출 → 캐시 배분.
