# Resume — HYE RAN LEE

HTML/CSS로 만든 포트폴리오 이력서.  
**"보기 좋은" 수준에서 접근성 · 시맨틱 · 유지보수까지 갖춘 코드로.**

🔗 **https://hyeramee.github.io/resume2026/**

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=flat-square&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=flat-square&logo=css3&logoColor=white)
![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-222222?style=flat-square&logo=github&logoColor=white)

---

## 왜 만들었나

포트폴리오용 이력서를 Figma로 디자인하고 직접 구현했다.  
만들면서 화면보다 코드 품질이 중요하다는 걸 느꼈고, 이 프로젝트는 그 간격을 메운 기록이다.

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

## 웹 접근성

### 키보드 접근성

`<span>` 은 Tab 키로 포커스가 잡히지 않고, 스크린리더가 역할을 인식하지 못한다.  
모바일 메뉴를 `<button>` 으로 교체하고 Reset CSS로 기본 스타일을 제거했다.

```html
<!-- Before -->
<span class="nav__menu-mobile">MENU</span>

<!-- After -->
<button class="nav__menu-mobile" type="button" aria-label="메뉴 열기">
  MENU
</button>
```

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
- [x] `lang="ko"` — 문서 언어 선언
- [x] W3C Markup Validator **Errors 0**

---

## 구현 포인트

### 플립카드 3D 애니메이션

`will-change: transform, opacity` 로 GPU 레이어를 미리 분리해 성능을 최적화했다.  
호버 시 `animation-play-state: paused` 로 자동 회전을 일시정지한다.

### 스크롤 형광펜 — JS 없이 CSS만

`<mark>` 태그로 강조 의미를 부여하고, `background-position` 이동으로 왼쪽에서 오른쪽으로 그어지는 효과를 구현했다.

### 글래스모피즘 내비게이션

선형 그라디언트 + `backdrop-filter: blur(16px)` 로 반투명 유리 효과를 구현했다.  
`-webkit-backdrop-filter` 로 Safari 벤더 프리픽스까지 챙겼다.

### CSS 변수로 디자인 토큰 관리

색상·반경·높이를 `:root` 한 곳에서 관리해 한 줄만 바꿔도 전체에 반영된다.  
`!important` **0번** — specificity 만으로 스타일 충돌 없이 해결했다.

### px → rem 단위 전환

`px` 는 사용자 브라우저 글꼴 설정을 무시한다. `rem` 으로 전환해 접근성을 확보했다.  
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

## 문제 및 해결

**1. `conic-gradient` 브라우저 렌더링 불일치**  
Chrome과 Safari 결과물이 달랐다. PNG 이미지로 교체해 해결했고, 앞으로는 PNG보다 용량이 가볍고 브라우저 호환성이 좋은 WebP를 사용할 것이다.

**2. `<span>` 메뉴의 키보드 접근 불가**  
마우스로는 됐지만 Tab 키로 포커스가 잡히지 않았다. `<button>` 으로 교체하고 Reset CSS로 기본 스타일을 제거했다.

**3. `<button>` 기본 스타일이 디자인을 깨는 문제**  
`<span>` → `<button>` 교체 후 테두리·배경이 생겨 디자인이 틀어졌다. Reset CSS로 초기화하고 스타일을 별도로 입혔다.

**4. 시맨틱 태그 교체 후 디자인 깨짐**  
`<span>` 을 `<h2>` 로 바꾸자 브라우저 기본 스타일로 글자가 커지고 굵어졌다. `font-size: inherit`, `font-weight: inherit` 으로 초기화해서 해결했다.

**5. W3C 마크업 유효성 오류**  
눈으로 보기엔 정상이었지만 Validator를 돌리니 `</li>` 종료 태그 위치 오류 1건이 나왔다. 브라우저가 자동 복구해줘서 화면은 멀쩡했던 것이다. 태그 구조 수정 후 Errors 0 통과.

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
