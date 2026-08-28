# TownPulse — 충청북도 마을생존 AI 의사결정 플랫폼

팀 **Pulse Lab**의 프로젝트 문서·포트폴리오 사이트입니다. 충북 153개 읍·면·동이 사라지는 과정을
19종 공공데이터와 ML 5종 모델로 추적하고, 소버린 AI(Exaone 3.5:7.8b 로컬)가 근거 있는 행정 처방과
예산 시뮬레이션을 제공합니다.

- 데모 사이트: <https://townpulse.site>
- 문서 사이트: <https://townpulse.beyondfacade.cloud>
- 참고(같은 팀): <https://beyondfacade.github.io/metabole.beyondfacade.cloud/>

## 로컬 실행

```bash
bundle install
bundle exec jekyll serve
# http://127.0.0.1:4000
```

## 구조

- `_config.yml` — Just the Docs 테마 · `townpulse` 컬러 스킴 · mermaid · 검색 설정
- `index.markdown` — 랜딩(표지). 히어로·핵심 지표·세 가지 축·팀·프로젝트 정보
- `docs/01~14` — 문제 인식부터 기술 요약까지 14개 문서 페이지(사이드바 nav)
- `_sass/color_schemes/townpulse.scss` — 파인 잉크 + 시빅 틸 팔레트
- `_sass/custom/custom.scss` — 랜딩 컴포넌트·eli5 콜아웃·인쇄(PDF) 스타일
- `_includes/head_custom.html` — MathJax(수식) · 개발 세션 배너
- `.github/workflows/jekyll.yml` — main 푸시 시 GitHub Pages 자동 배포

## 배포

`main` 브랜치에 푸시하면 GitHub Actions가 빌드해 GitHub Pages로 배포합니다.
저장소 **Settings → Pages → Source**를 **GitHub Actions**로 설정하세요.
