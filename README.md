# 자바웹프로그래밍(2) 실습

## 3주차 - 포트폴리오 작성하기 완료
- 템플릿: [TemplateMo 578 First Portfolio](https://templatemo.com/tm-578-first-portfolio) (Bootstrap 5.1.3, 원페이지)
- [index.html : 실행 / 수정 완료](demo/src/main/resources/templates/index.html)
  - 기존 index.html은 [index_backup.html](demo/src/main/resources/templates/index_backup.html)로 백업
  - css, js, images, fonts → `resources/static`으로 이동
  - 자원 경로를 타임리프 URL 방식으로 수정: `(href|src)="(css|js|images)(/[^"]*)"` → `th:$1="@{/$2$3}"`, 로고 링크 `th:href="@{/}"`
  - 네비게이션 메뉴 한글화: 홈페이지 / 소개 / 기술 / 프로젝트 / 연락처
  - 한글 글씨 크기 수정: [templatemo-first-portfolio-style.css](demo/src/main/resources/static/css/templatemo-first-portfolio-style.css) (`--menu-font-size` 12px → 20px, `--p-font-size` 18px → 20px)
  - 폼 버그 수정: label의 `for`와 input의 `id` 불일치 (원본 497, 505, 557번 줄) → `name`, `email`, `message`로 일치
- 프로필 수정: 소개(관심 있는 것 / 잘하는 것 / 하고 싶은 것), 기본 정보, 프로필 사진 `static/images/profile.png`
- 기술(Services) 4가지: 웹, AI, 보안, 데이터
  - 아이콘: `bi-globe`, `bi-anthropic`, `bi-shield-lock`, `bi-bar-chart-line` (최신 Bootstrap Icons CDN 추가)
- 상세 페이지: `resources/public` 폴더 (컨트롤러 없이 바로 접근)
  - `<a th:href="@{/detailed_web.html}" target="_blank" rel="noopener noreferrer">`
  - noopener: 새 창이 `window.opener`로 원래 페이지를 조작하지 못하게 막음 / noreferrer: 어디서 왔는지(Referer) 안 넘김

## 3주차 연습문제
### 1. 포트폴리오 수정하기
- 나의 4가지 영역(웹, AI, 보안, 데이터) 아이콘 수정 완료
- 상세 페이지 4개 추가 (detailed_web.html 재활용)
  - [detailed_web.html](demo/src/main/resources/public/detailed_web.html)
  - [detailed_ai.html](demo/src/main/resources/public/detailed_ai.html)
  - [detailed_security.html](demo/src/main/resources/public/detailed_security.html)
  - [detailed_data.html](demo/src/main/resources/public/detailed_data.html)

### 2. 성능 확인 - 원인 분석 (Lighthouse)
- PC보다 모바일 Performance 점수가 낮게 나온다.
- **차이가 나는 이유**: Lighthouse 모바일 검사는 느린 4G 네트워크와 4배 느린 CPU를 흉내 내서 검사하기 때문에, 같은 페이지라도 파일을 받고 실행하는 시간이 훨씬 길게 잡힌다.
- **성능을 가장 떨어뜨리는 항목 2개**
  1. **렌더링 차단 리소스**: `<head>`의 Google Fonts, bootstrap.min.css, 아이콘 CSS를 다 받기 전까지 화면을 못 그린다. → 폰트 `preload`, 필요한 CSS만 쓰기
  2. **사용하지 않는 JavaScript/CSS**: jQuery, magnific-popup 등 큰 파일을 페이지 시작 때 모두 불러오고, 프리로더(로딩 화면)가 끝날 때까지 첫 화면이 늦게 보인다. → `defer`, 안 쓰는 라이브러리 정리

## 2주차 - 스프링 부트 개발환경, 테스트 완료
- [2주차 저장소](https://github.com/LSY0820/springboot-week2) 코드를 이어서 사용 (`/hello`, `/hello2` 그대로 동작)

### 실행 방법
`demo/src/main/java/com/example/demo/DemoApplication.java` 열고 `Ctrl + F5` → http://localhost:8080
