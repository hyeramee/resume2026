# Resume — HYE RAN LEE

> HTML/CSS로 만든 포트폴리오 이력서.  
> **"보기 좋은" 수준에서 접근성 · 시맨틱 · 유지보수까지 갖춘 코드로.**

🔗 **https://hyeramee.github.io/resume2026/**

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?style=flat-square&logo=github&logoColor=white)

---

## 왜 만들었나

이력서 자체가 개발 역량을 증명할 수 있다고 생각했다.  
디자인은 있는데 코드 품질이 없으면 반쪽짜리다.  
이 프로젝트는 그 간격을 직접 메운 기록이다.

---

## 시맨틱 HTML

`<div>` 와 `<span>` 으로 된 마크업을 의미 있는 태그로 교체했다.

| 변경 전 | 변경 후 | 이유 |
|---|---|---|
| `<strong>` | `<span>` + CSS `font-weight` | 시각적 강조와 의미적 강조는 다르다 |
| `<span>` 제목 | `<h2>` | 문서 계층 구조 명확화 |
| `<span>` 메뉴 | `<button type="button">` | 키보드 · 스크린리더 접근성 |

시맨틱 태그가 기존 디자인을 깨지 않도록 `h1, h2, h3` 의 기본 크기·굵기를 `inherit` 으로 초기화했다.

```css
h1, h2, h3 {
  font-size: inherit;
  font-weight: inherit;
}
```

W3C Markup Validator 로 전체 마크업 검사 → 짝이 맞지 않는 `</li>` 1건 수정 후 **Errors 0** 통과.

---

## 웹 접근성 (A11y)

### 키보드 접근성

`<span>` 으로 구현된 모바일 메뉴를 `<button>` 으로 교체했다.  
`<span>` 은 Tab 키로 포커스가 잡히지 않고, 스크린리더가 역할을 인식하지 못한다.

```html
<!-- Before -->
<span class="nav__menu-mobile">MENU</span>

<!-- After -->
<button class="nav__menu-mobile" type="button" aria-label="메뉴 열기">
  MENU
</button>
```

`<button>` 의 기본 스타일(테두리, 배경)은 Reset CSS 로 제거하고 디자인을 별도로 입혔다.

```css
button {
  background: none;
  border: none;
  cursor: pointer;
  font: inherit;
  color: inherit;
  padding: 0;
}
```

### 접근성 체크리스트

- [x] `<button type="button">` + `aria-label="메뉴 열기"` — 스크린리더 역할 안내
- [x] `<img>` 에 `alt`, `width="186" height="186"` 명시 — CLS 방지
- [x] `word-break: keep-all` — 한국어 단어 중간 잘림 방지
- [x] `rel="noopener noreferrer"` — 외부 링크 보안
- [x] `lang="ko"` — 문서 언어 선언
- [x] W3C Markup Validator **Errors 0**

---

## 구현 포인트

### 플립카드 3D 애니메이션

```css
.flip-card {
  transform-style: preserve-3d;
  animation: auto-flip 5s ease-in-out infinite;
  will-change: transform, opacity; /* GPU 레이어 분리 */
}
.flip-card__face {
  backface-visibility: hidden;
  -webkit-backface-visibility: hidden;
}
.flip-scene:hover .flip-card {
  animation-play-state: paused; /* 호버 시 일시정지 */
}
```

`will-change` 로 GPU 레이어를 미리 분리해 애니메이션 성능을 최적화했다.  
메탈릭 뒷면은 `conic-gradient` 브라우저 렌더링 차이로 인해 PNG 이미지로 교체해 픽셀 정밀도를 확보했다.

---

### 스크롤 형광펜 — JS 없이 CSS만

```css
mark.scroll-highlight span {
  background: linear-gradient(50deg, var(--bg-color) 50%, transparent 50%)
    110% 0 / 200% 100% no-repeat;
  animation: highlightDraw 5s cubic-bezier(0.22, 0.61, 0.36, 1) 1s both;
}

@keyframes highlightDraw {
  from { background-position: 110% 0; }
  to   { background-position: 0% 0; }
}
```

`<mark>` 태그로 강조 의미를 부여하고, `background-position` 이동으로 왼쪽에서 오른쪽으로 그어지는 효과를 구현했다.  
`--bg-color` CSS 변수로 다크모드 색상 분기도 처리했다.

---

### 글래스모피즘 내비게이션

```css
.nav {
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px); /* Safari 대응 */
  background: linear-gradient(
    to right,
    rgba(236, 230, 222, 0.20),
    rgba(245, 245, 247, 1.00)
  );
}
```

`-webkit-backdrop-filter` 로 Safari 벤더 프리픽스까지 챙겼다.

---

### CSS 변수로 디자인 토큰 관리

```css
:root {
  --color-bg:     #fffaf2;
  --color-pink:   #fdd6f5;
  --color-beige:  #ece6de;
  --radius-card:  26px;
  --nav-height:   3rem;
  --border-width: 0.8px;
}
```

색상·반경·높이를 `:root` 한 곳에서 관리.  
`!important` **0번** — specificity 만으로 스타일 충돌 없이 해결했다.

---

### px → rem 단위 전환

| 항목 | 변경 전 | 변경 후 |
|---|---|---|
| 기본 폰트 | `14px` | `0.875rem` |
| 대제목 | `46px` | `2.875rem` |
| 패딩 | `34px` | `2.125rem` |
| 갭 | `26px` | `1.625rem` |
| 내비 높이 | `48px` | `3rem` |

브라우저 기본 폰트(16px) 기준 상대 단위로 전환해 사용자 글꼴 설정을 존중한다.  
고정 레이아웃(카드 `937px`, 컬럼 `434px`)과 서브픽셀 보더(`0.8px`)는 의도적으로 `px` 유지.

---

## 반응형

| 브레이크포인트 | 환경 | 변화 |
|---|---|---|
| `1280px` | 데스크탑 와이드 | 카드 최대 너비 기준 2열 |
| `1080px` | 데스크탑 기본 | 2열 유지, 컬럼 간격 조정 |
| `768px` | 태블릿 | 1열 전환, 풀 와이드 |
| `320px` | 모바일 최소 | 메뉴 접힘, 사진 `order: -1` 로 텍스트 위 배치 |

---

## 브랜치 전략

```
feat-interaction-2   ──┐
                        ├── merge ──▶  main ──▶ GitHub Pages 자동 배포
feat/semantic-a11y   ──┘
```

---

## 파일 구조

```
📁 resume
├── index.html            # 시맨틱 HTML + 접근성 속성
├── style.css             # CSS 변수 · Reset · 컴포넌트 · 미디어 쿼리
└── silver_card_back.png  # 메탈릭 뒷면 이미지
```

---

<sub>2026.06 · HYE RAN LEE · Frontend Developer</sub>
