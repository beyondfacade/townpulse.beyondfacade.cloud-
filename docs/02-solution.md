---
title: 2. 솔루션 개요와 차별성
layout: default
nav_order: 2
---

# 솔루션 개요와 차별성
{: .no_toc }

<div class="eli5" markdown="1">
<span class="eli5-label">IN PLAIN ENGLISH</span>

TownPulse combines 19 public datasets into one engine that scores each town's survival (TVI v2.1) across population, vacancy, transport, and infrastructure, then has a locally-hosted sovereign AI write budget-backed prescriptions — no sensitive data ever leaves the server. The nearest competitor, "MyVillage AI," is a chatbot for would-be movers covering one city (Jecheon); TownPulse is a decision dashboard for officials covering all 153 towns, with ML forecasts and budget simulation. They complement each other: MyVillage AI tells residents *where to go*, TownPulse tells governments *which town to save first*.
</div>

## 2-1. 솔루션 한 줄

> 충북 153개 읍면동이 사라지는 과정을 **19종 공공데이터와 ML 5종 모델**로 추적하고, **소버린 AI**가 지자체에 근거 있는 행정 처방과 예산 시뮬레이션을 제공하는 AI 행정 의사결정 보조 시스템.

- **19종 공공 API** 결합·분석 AI 엔진 — 전 API 개발계정 승인 및 probe 검증 완료
- 인구·빈집·교통·인프라 4대 부문 복합 지수(**TVI v2.1**) + 5대 동적 가중치 시나리오 자동 산출
- **Exaone 3.5:7.8b 소버린 AI** — Fly.io 서버 내 Ollama 완전 로컬 호스팅으로 민감 행정 데이터 외부 전송 없음, SSE 스트리밍 컨설팅
- **ML 5종 모델** — 153마을 × 36개월 = 5,508행 실데이터 훈련 완료
- **실정책 단가 기반 예산 시뮬레이션** — 농식품부·행안부·국토부 공식 사업 단가 9건, 재정자립도 기반 국비/지방비 자동 계산
- 대시보드·A4 PDF 리포트로 행정 실무 즉시 활용

## 2-2. 내마을AI와의 차별성 (심사 예상 질문 대응)

2025년 7월 충북도·제천시가 국토부 스마트도시 데이터허브 사업(20억, 국비 10억+지방비 10억)으로 **내마을AI(LLM 대화형 정보서비스)**를 구축 중입니다.

| 구분 | 내마을AI | TownPulse |
|---|---|---|
| 대상 사용자 | 전 국민 (귀촌 희망자) | 지자체 공무원 (행정 담당자) |
| 핵심 목적 | 이주·귀촌 정보 안내 | 소멸 조기경보 + 행정 의사결정 보조 |
| 커버 범위 | 제천시 단일 | 충북 전체 **153개 읍면동** |
| 접근 방식 | 챗봇 대화형 | 히트맵 + 복합 지수 + AI 처방 대시보드 |
| 핵심 기능 | 질문 응답 | 위험 마을 조기경보 + **ML 예측 + 예산 시뮬레이션** |
| 데이터 관점 | 생활인구 중심 | 인구·빈집·교통·인프라 19종 복합 |
| AI 원칙 | 외부 LLM API | **소버린 AI** — Ollama 로컬 호스팅 |

> **상호보완:** "내마을AI는 귀촌 희망자가 *어디로 갈지* 안내하고, TownPulse는 지자체가 *어느 마을을 먼저 살릴지* 결정을 돕습니다. 두 서비스는 경쟁이 아니라 정책 실행의 앞뒤 단계를 담당합니다."

**직접 경쟁 분석 요약**

| 서비스 | 주체 | 차이점 |
|---|---|---|
| 빈집정비 통합지원시스템 (binzibe.kr) | 한국부동산원 | 빈집 단일 문제만, 교통·인구·인프라 연계 없음 |
| 내마을AI (개발 중) | 충북도+제천시 | 귀촌 안내 챗봇 수준, ML 예측·처방 자동 생성 미지원 |
| KT PLIP | KT | 통신 빅데이터 기반, 지자체 맞춤 처방 없음 |
| 뉴엔AI 퀘타아이 | 뉴엔AI | 범용 플랫폼, 빈집·교통·인프라 통합 없음 |
