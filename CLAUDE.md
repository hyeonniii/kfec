# CLAUDE.md

이 파일은 Claude Code가 이 저장소에서 작업할 때 참고할 안내입니다.

## 프로젝트 개요

한국법과학원(KFEC, Korean Forensic Experts & Consulting) 홈페이지 소스입니다.
빌드 도구, 패키지 매니저, JS 프레임워크가 전혀 없는 **순수 정적 HTML/CSS 사이트**입니다
(`package.json` 없음). `CNAME` 파일(`kfec.co.kr`)로 미루어 GitHub Pages로 배포되는 것으로
보입니다.

## 실행 / 로컬 확인 방법

빌드 과정이 없습니다. 다만 각 페이지가 `header.html`/`sidenav.html`/`footer.html`을
JS `fetch()`로 불러오기 때문에, HTML 파일을 `file://`로 직접 더블클릭해서 열면 브라우저
보안 정책(CORS)에 막혀 헤더/사이드바/푸터가 비어 보입니다. 로컬에서 확인할 때는 항상
간단한 정적 서버를 띄우고 접속하세요.

```bash
python3 -m http.server 8000
# 이후 http://localhost:8000/index.html 접속
```

## 파일 구조

- `index.html` / `index_en.html` — 홈 (한국어 / 영어)
- `ae.html` — 전문 서비스 분야
- `pp.html` — 전문가 약력
- `contact.html` — 서비스 의뢰
- `header.html`, `footer.html` — 모든 페이지 공통 조각. 단독으로는 렌더링되지 않고
  각 페이지의 인라인 `<script>`가 `fetch()`로 불러와 `#header`/`#footer`에 주입합니다.
- `sidenav.html` (한국어 페이지용) / `sidenav_en.html` (영어 페이지용) — 좌측 사이드
  내비게이션. 상단 KR/EN 버튼이 `location.href`로 언어 페이지를 전환합니다. 마찬가지로
  `fetch()`로 `#sidenav`에 주입되는 방식입니다.
- `styles.css` — 전역 스타일 전부가 이 한 파일에 있습니다. 구글 폰트(`42dot Sans`)를
  CDN에서 불러와 기본 폰트로 사용합니다.
- `logo.png`, `service.png`, `service2.png`, `fire.jpg`, `img1~3.jpg`, `intro1~2.jpg`,
  `english.pdf` — 루트에 직접 있는 이미지/문서 에셋.
- `index3.html` — 10줄짜리 파일로 다른 페이지에서 링크되지 않는 것으로 보이는
  미사용/레거시 파일입니다. 임의로 삭제하지 말고, 필요 시 사용자에게 확인하세요.

## 알아둘 관례

- **공통 조각 수정**: 헤더/푸터/사이드바를 바꾸려면 `header.html` / `footer.html` /
  `sidenav.html` (`sidenav_en.html`)만 고치면 이를 불러오는 모든 페이지에 반영됩니다.
- **신규 페이지 추가**: 새 HTML 페이지를 만들 때는 기존 페이지(`index.html` 등)에 있는
  header/sidenav/footer `fetch()` 로딩 스크립트 3개를 그대로 복사해 넣어야 레이아웃이
  동일하게 나옵니다.
- **한/영 이중 페이지 쌍**: 콘텐츠 페이지는 `xxx.html`(한국어) / `xxx_en.html`(영어)
  파일명 규칙으로 쌍을 이룹니다. 사이드바 텍스트도 `sidenav.html`/`sidenav_en.html`로
  따로 존재하므로, 한쪽 언어만 수정하고 다른 언어 페이지를 빠뜨리기 쉽습니다 — 내용을
  고칠 때 두 언어 버전을 함께 확인하세요.
- 코드 주석과 본문 텍스트는 한국어가 기본입니다.
- 일부 페이지에 인라인 `style="..."` 속성이 남아 있어, 전역 스타일(`styles.css`)과
  인라인 스타일이 혼재되어 있습니다.
