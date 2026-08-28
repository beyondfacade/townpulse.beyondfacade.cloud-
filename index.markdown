---
layout: default
title: 표지
nav_exclude: true
---

<div class="landing">

  <!-- ── 히어로 ── -->
  <section class="hero">
    <p class="hero-eyebrow">TEAM PULSE LAB · 창업경진대회 포트폴리오</p>
    <h1 class="hero-title">
      마을이 사라지는 과정을,<br>
      지도 위에서 <span class="accent">먼저 읽습니다</span>
    </h1>
    <p class="hero-lead">
      빈집이 늘면 마을이 비고, 마을이 비면 버스가 끊기고, 버스가 끊기면 남은 주민이 고립됩니다.
      <strong>TownPulse</strong>는 이 악순환을 충청북도 153개 읍·면·동에서 짚어냅니다.
      19종 공공데이터와 ML 5종 모델로 소멸의 속도를 추적하고, 서버 안에서만 도는 소버린 AI가
      "어느 마을을 먼저, 어떤 처방으로, 예산은 얼마나" 답을 정리해 지자체 담당자에게 건넵니다.
    </p>
    <div class="hero-actions">
      <a class="btn-cta primary" href="https://townpulse.site">지도에서 진단하기</a>
      <a class="btn-cta ghost" href="{{ '/docs/01-problem.html' | relative_url }}">프로젝트 문서</a>
      <a class="btn-cta ghost" href="https://github.com/beyondfacade/townpulse.beyondfacade.cloud-">GitHub</a>
    </div>

    <div class="eli5">
      <span class="eli5-label">IN PLAIN ENGLISH</span>
      <p>Rural Chungbuk is emptying out — empty houses, shrinking population, and disappearing bus
      routes that all feed each other. TownPulse pulls 19 public datasets across 153 towns, scores
      each town's survival index from 0 to 100, forecasts where it is heading with 5 ML models, and
      lets a locally-hosted AI recommend concrete fixes with real budget figures and funding sources.
      It is a decision tool for local government officials, not a chatbot for residents.</p>
    </div>

    <div class="stat-band">
      <div class="stat">
        <p class="stat-number">153<span class="unit">개</span></p>
        <p class="stat-label">충북 전 읍·면·동<br>"리" 단위 배제(방안 B)</p>
      </div>
      <div class="stat">
        <p class="stat-number">19<span class="unit">종</span></p>
        <p class="stat-label">공공 API 승인·검증 완료<br>2026-06-19 ~ 2028-06-19</p>
      </div>
      <div class="stat">
        <p class="stat-number">5,508<span class="unit">행</span></p>
        <p class="stat-label">실데이터 훈련 샘플<br>153마을 × 36개월</p>
      </div>
      <div class="stat">
        <p class="stat-number">5<span class="unit">종</span></p>
        <p class="stat-label">ML 예측 모델 훈련 완료<br>XGBoost·Prophet·K-Means·Ridge</p>
      </div>
    </div>
  </section>

  <!-- ── 세 가지 축 ── -->
  <section class="landing-section">
    <h2>이 프로젝트가 증명하는 것</h2>
    <p class="section-sub">수집부터 분석·예측·서빙까지, 데이터 프로덕트의 전 과정을 팀으로 완주합니다</p>
    <div class="value-grid">
      <div class="value-card">
        <p class="value-icon">📊</p>
        <h3>데이터 엔지니어링</h3>
        <p>행안부·국토교통부·통계청·소상공인공단·국립중앙의료원·KERIS까지 공공 API 19종을 한 파이프라인으로 모읍니다.
        시군구 단위로 캐싱하고 병렬로 호출해 쿼타를 아끼며, 좌표계와 법정동코드를 맞춰 넣습니다.
        해시나 난수로 지어낸 가짜 값은 DB에 넣지 않습니다.</p>
      </div>
      <div class="value-card">
        <p class="value-icon">🧮</p>
        <h3>TVI v2.1 · 소버린 AI</h3>
        <p>인구·빈집·교통·인프라 네 부문과 다섯 가지 동적 시나리오로 마을생존지수를 0~100점으로 매깁니다.
        행정·전력·통계·실거래를 겹쳐 실질 빈집율을 잡아내고, 서버 안에서만 도는 Exaone 3.5:7.8b가
        예산 범위와 기금 출처에 근거를 달아 처방을 SSE로 스트리밍합니다.</p>
      </div>
      <div class="value-card">
        <p class="value-icon">🏛️</p>
        <h3>헥사고날 프랙탈 아키텍처</h3>
        <p>모듈러 모놀리스 위에 헥사고날(Ports &amp; Adapters)과 클린 아키텍처, DDD를 얹었습니다.
        도메인마다 파일 11개를 1:1로 맞춘 프랙탈 구조라 AI가 코드를 다루기 쉽고, 경계에서 매퍼가 타입을 걸러
        도메인을 깨끗하게 지킵니다. 어댑터만 갈아 끼우면 충북에서 전국으로 넓힙니다.</p>
      </div>
    </div>
  </section>

  <!-- ── 팀 ── -->
  <section class="landing-section">
    <h2>팀 — Pulse Lab</h2>
    <p class="section-sub">전원이 백엔드(FastAPI)·프론트엔드(Next.js)를 함께 짠 풀스택 3인 체제</p>
    <div class="team-grid">
      <div class="team-card">
        <div class="team-avatar">김</div>
        <p class="team-name">김충식</p>
        <p class="team-role">팀장 · PM<br>아키텍처 총괄 · 공공 API 검증</p>
      </div>
      <div class="team-card">
        <div class="team-avatar">이</div>
        <p class="team-name">이은상</p>
        <p class="team-role">풀스택 · QA<br>정합성·할루시네이션 검증</p>
      </div>
      <div class="team-card">
        <div class="team-avatar">신</div>
        <p class="team-name">신채연</p>
        <p class="team-role">풀스택 · 해외 전략<br>지도·시각화 · AI UX · PDF 리포트</p>
      </div>
    </div>
  </section>

  <!-- ── 프로젝트 정보 ── -->
  <section class="landing-section">
    <h2>프로젝트 정보</h2>
    <p class="section-sub">충청북도 마을생존 AI 의사결정 플랫폼</p>
    <div class="info-strip">
      <dl>
        <dt>서비스 · 팀명</dt>
        <dd>TownPulse · Pulse Lab</dd>
      </dl>
      <dl>
        <dt>대상 지역</dt>
        <dd>충청북도 153개 읍·면·동 (11개 시군)</dd>
      </dl>
      <dl>
        <dt>대회</dt>
        <dd>2026 충청북도 공공데이터·AI 활용 창업경진대회 (제품·서비스 개발)</dd>
      </dl>
      <dl>
        <dt>핵심 산출물</dt>
        <dd>마을생존지수(TVI v2.1) · 소버린 AI 5개년 처방 · 예산 시뮬레이션</dd>
      </dl>
      <dl>
        <dt>깃허브</dt>
        <dd><a href="https://github.com/beyondfacade/townpulse.beyondfacade.cloud-">github.com/beyondfacade/townpulse.beyondfacade.cloud-</a></dd>
      </dl>
      <dl>
        <dt>데모 사이트</dt>
        <dd><a href="https://townpulse.site">townpulse.site</a></dd>
      </dl>
    </div>
  </section>

  <p class="landing-footnote">본 사이트는 TownPulse 프로젝트의 개발 문서이자 팀 포트폴리오입니다.</p>

</div>
