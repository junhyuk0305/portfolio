# 포트폴리오 디자인 20종 — Gemini AI Studio 프롬프트 모음

> Part 1 '데이터 정제'에서 만든 브랜딩 명세서와 본문 텍스트를 Part 2 바이브 코딩으로
> 옮겨 담기 위한 20개의 독립 프롬프트 모음입니다. 각 프롬프트는 자체 완결형이며,
> 본인 브랜딩 명세서와 가장 잘 맞는 1종을 골라 Gemini AI Studio에 그대로 붙여 넣고
> [변수] 자리만 채우면 됩니다.

---

## 사용법

1. Part 1 산출물(브랜딩 명세서 + 본문 텍스트)을 손에 든다.
2. 아래 20종 중 본인 명세서의 무드 / 컬러 톤과 가장 잘 맞는 1종을 고른다.
3. 해당 디자인의 Prompt 블록을 그대로 복사 → Gemini AI Studio 채팅창에 붙여 넣는다.
4. 프롬프트 끝의 "Insert content" 블록 자리에 본인 텍스트를 그대로 붙여 전송한다.
5. 결과를 Part 2 4-STEP 폴리싱 → Part 3 GitHub Pages 배포 흐름으로 마감.

## 공통 규칙

아래 20종 프롬프트 모두에 공통으로 적용되는 규칙입니다. Gemini AI Studio에 프롬프트를
던질 때, 선택한 디자인 프롬프트 위/아래에 다음 3가지를 함께 명시하세요.

- **단일 HTML 파일에서 개발하도록.** 모든 마크업·스타일·스크립트를 하나의 `.html` 파일
  안에 인라인으로 작성합니다 (외부 CSS / JS 파일 분리 금지). Tailwind는 CDN으로 로드,
  폰트는 Google Fonts 등 CDN 링크로 head에 포함합니다. GitHub Pages에 파일 하나만
  올리면 바로 배포되도록 하기 위함입니다.
- **본문 텍스트는 무조건 한국어로.** 프롬프트 자체는 영어로 작성되어 있지만, 결과물의
  Hero 카피·프로필·프로젝트 본문·섹션 라벨·버튼 등 사용자에게 노출되는 모든 텍스트는
  한국어로 렌더링되어야 합니다. (영문 디자인 용어가 들어간 라벨도 한국어로 의역)
- **공통 변수 필요 없어.** `[HERO_COPY]` / `[PROFILE_TEXT]` 같은 플레이스홀더를 따로
  치환하는 단계 없이, 본인의 person.md 텍스트를 "Insert content" 블록 자리에 통째로
  붙여 넣으면 됩니다. Gemini AI Studio가 알아서 적절한 섹션에 배치합니다.

## 공통 규칙 — v2 추가사항 (모든 디자인 필수)

아래 3개 항목은 20종 디자인 **모두에 강제 적용**되는 v2 규칙입니다. 각 디자인의
고유한 motion 스펙은 그대로 살리되, 아래 규칙들이 **위에 덧붙여집니다.** 디자인의
정체성을 해치지 않는 선에서 모든 디자인이 동일한 메커니즘을 공유해야 합니다.

### 1) 스크롤 트리거 애니메이션 (필수)

모든 디자인은 IntersectionObserver 기반 스크롤 트리거 모션을 **기본값으로 반드시
포함**합니다. 단순한 fade-in을 넘어, 다음 3종을 페이지에 최소 1번씩은 등장시키세요:

- **a. 진입 페이드(필수)** — 모든 섹션·카드·이미지가 viewport에 25% 들어왔을 때
  `opacity 0→1 + translateY 32px→0` (300~500ms, ease-out). 한 번 트리거되면
  unobserve.
- **b. 마스크 리빌(필수)** — 헤드라인 또는 키 이미지에 `clip-path: inset(0 100% 0 0)
  → inset(0 0 0 0)` 또는 세로 방향 reveal (700ms, cubic-bezier(.7,0,.3,1)).
- **c. 텍스트 스플릿(필수)** — 최소 1개의 큰 제목을 단어 또는 글자 단위로 stagger
  reveal (각 50~80ms 간격). 디자인 톤에 맞게 fade / blur-up / slide 중 선택.
- **d. 페이지 상단 1px 진행 바(필수)** — `position: fixed; top: 0;` 의 1px 라인이
  스크롤 진행률을 표시. 색은 디자인의 액센트 컬러 사용.
- **e. 이미지 Ken Burns 또는 패럴랙스(필수)** — 모든 이미지는 등장 시 scale
  1.08→1.0 (Ken Burns) 또는 scroll-linked translateY 패럴랙스 중 하나를 적용.

구현은 외부 라이브러리 없이 vanilla JS + IntersectionObserver + requestAnimationFrame
으로. `prefers-reduced-motion: reduce` 가 켜져 있으면 모션은 비활성화하되 상태는
즉시 final-state로 출력.

### 2) 색감 토글 (라이트 ↔ 다크) (필수)

모든 디자인은 **우측 상단 fixed 위치에 색감 토글 버튼**을 둡니다. 클릭 시 디자인
원본 팔레트(primary)와 **대안 팔레트(alt)** 간 즉시 스왑되어야 합니다.

- **구현**: 팔레트는 `:root` 의 CSS 변수로 정의하고, `data-theme="alt"` 속성을
  `<html>` 또는 `<body>` 에 토글. 전환 시 모든 색은 250ms ease로 부드럽게.
- **저장**: 선택은 `localStorage.setItem('tk-theme', 'primary' | 'alt')` 로 기억.
  초기 로드 시 저장값을 읽어 적용. 미저장 시 디자인의 기본 팔레트(primary).
- **버튼 위치**: top-right, 32px 떨어진 곳. 색감 토글 버튼은 디자인 톤에 맞춰
  최소한으로 (작은 원형 아이콘 또는 텍스트 라벨 "LIGHT / DARK" 또는 "01 / 02").
- **alt 팔레트 가이드** (디자인별):
  - 다크 베이스(`#09`, `#17`, `#11`, `#14` 등) → alt는 동일 액센트를 유지하되
    배경/텍스트를 라이트로 반전.
  - 라이트 베이스(`#01`, `#03`, `#15` 등) → alt는 동일 액센트 유지, 배경/텍스트
    를 다크로 반전.
  - 컬러 베이스(`#12`, `#13`, `#16`) → alt는 보색 또는 정반대 채도(예: #12
    fluoro yellow → fluoro magenta/cyan, #15 파스텔 → 차콜 베이스+같은 파스텔).
  - `#19` Scroll Cinema 처럼 챕터별 팔레트인 경우 → primary는 원본 챕터 톤,
    alt는 전체 챕터를 라이트(또는 다크) 한 톤으로 재구성.
- 토글 후에도 디자인의 **정체성**(타이포·레이아웃·모션)은 변하지 않습니다.
  오로지 컬러만 스왑.

### 3) 이미지 활용 (필수, 4장 고정)

`designs/png/` 폴더의 4개 이미지를 **모든 디자인이 반드시 사용**합니다. 디자인의
톤에 맞춰 그레이스케일·세피아·콘트라스트·블렌드 모드로 필터링하되, 1장 이상은
원본 컬러를 살린 hero/cover 용도로 둡니다.

- `IMG_PORTRAIT` — `designs/png/Gemini_Generated_Image_ylvv0qylvv0qylvv.png`
  (도서관에서 노트북으로 작업 중인 1인) → Hero 포트레이트, About 사진, 또는
  자기소개 옆 thumbnail.
- `IMG_TEAM` — `designs/png/Gemini_Generated_Image_i6tldbi6tldbi6tl.png`
  (도서관에서 노트북 들고 팀 토론) → 프로젝트 1 RoomSync (학내 협업 · MVP
  회의) 또는 동아리 회장 리더십 카드의 커버.
- `IMG_LECTURE` — `designs/png/Gemini_Generated_Image_o20szmo20szmo20s.png`
  (대형 강당에서 발표) → 프로젝트 3 동아리 회장(서로의 언어로 말하기 세미나)
  또는 detail 페이지의 dramatic full-bleed plate.
- `IMG_INTERVIEW` — `designs/png/Gemini_Generated_Image_dsku3ydsku3ydsku.png`
  (시장에서 인터뷰) → 프로젝트 2 A사 커머스 인턴(사용성 테스트 · 현장 리서치)
  의 커버.

각 디자인의 이미지 처리 가이드:
- 미니멀(#01~04): 흑백 또는 70% 그레이스케일, 1px hairline frame.
- 에디토리얼(#05~08): 흑백 또는 듀오톤. 캡션 필수 (italic 또는 small caps).
- 다크 테크(#09~11): 다크 톤 + slight color shift, 카드 안에 inset.
- 브루탈리스트(#12~14): 거친 contrast, 4px black frame + hard shadow.
- 컬러풀(#15~16): 원본 컬러 그대로, rounded 16px corners.
- 럭셔리(#17~18): 세피아 또는 매우 어두운 cinematic dim, 풀-블리드.
- 모션(#19~20): 풀-블리드 + 패럴랙스 + Ken Burns.

**이미지 애니메이션 (필수)**: 모든 이미지에 다음 중 1개 이상 적용:
- Ken Burns (등장 시 scale 1.08→1.0, 1200ms)
- 마스크 리빌 (clip-path 위→아래 또는 좌→우)
- 패럴랙스 (scroll에 따라 translateY · 강도는 디자인 톤에 따라 0.1~0.3)
- 그레이스케일 → 컬러 전환 (viewport 진입 시 또는 hover 시)
- 다크 커튼이 위에서 아래로 사라지며 이미지 노출

## 공통 규칙 — v3 추가사항 (전 디자인 강제, 위반 시 빌드 실패)

### 1) 데이터 정확성 — Fake 금지 (최우선 규칙)

모든 본문·수치·역할·조직·인용·연도·정량 지표는 **person.md에 명시된 것만** 사용합니다.
시각적으로 자리를 채워야 할 곳에 person.md에 없는 정보를 임의 생성하는 것은
**절대 금지**입니다. 부족한 자리는 다음 중 하나로 처리:

- 빈 자리로 두고 디자인의 시각적 균형을 다른 요소로 맞춤
- "—" 또는 "n/a" 표시
- 일반화된 라벨만 (예: "KEY METRIC", "OUTCOME", "CASE")

특히 다음은 **자주 fake로 생성되는 위험 항목** — 반드시 person.md 본문에서만 발췌:
- MAU/DAU/WAU 수치 (예: "MAU 12k", "200명" 등 임의 추가 금지)
- 인터뷰 수, 멘토링 수, 사용자 수 (예: "30명 인터뷰", "40명 멘토링" 금지)
- 새 전환율 수치 (예: "+41% 매칭" 금지)
- 경력 연차 (예: "5년 경력" 금지 — person.md에 명시 없음)
- 회사명·프로젝트명·제품 기능 (RoomSync · A사 · 동아리 외 새 이름 만들지 말 것)

**person.md에서 사용 가능한 정량 지표 (전부 — 이 외는 사용 금지)**:
- RoomSync: 학내 50여 개 동아리 (모집단), 30개 동아리 가입 (런칭 결과), 노쇼율 60% 감소, 출시 1개월, 2023.07–2023.11
- A사 인턴: MAU 50만 (플랫폼 규모), 상세→장바구니 이탈률 45%, 사용자 5명 UT,
  장바구니 전환율 12%p 상승, 2023.01–2023.06
- 동아리 회장: 100명 규모 연합 동아리, 기존 프로젝트 완성률 40%, 데모데이 배포율 85%, 월 1회 세미나, 2022.03–2022.12
- 전공: 경영학+컴퓨터공학 복수전공, 학점 3.9/4.5
- 스킬: Notion, Figma(기본), GA4, SQL, Agile 스크럼 리드

### 2) 가독성 — 대비·색 충돌·텍스트 가시성

- 본문 텍스트 ↔ 배경 대비 WCAG AA(4.5:1) 보장. 헤딩은 3:1 이상.
- 보색(예: 빨강↔초록, 노랑↔파랑) 또는 채도 충돌 위에 직접 본문 텍스트 배치 금지.
- 이미지 위 텍스트는 반드시 dark overlay (linear-gradient or solid mask) 추가.
- 액센트 색은 "포인트"로만 사용. 본문(15px 이상 문단) 색으로는 금지.
- 한글 본문 최소 15px (모바일 14px), line-height 1.55 이상.
- 토글·버튼: hover 상태와 default 상태 모두 식별 가능해야 함 (단순 색상 변화로는 부족, 보더/언더라인 등 추가).
- 다크 모드에서 회색 #888 이하의 muted 색은 본문에서 사용 금지 (대비 미달).

### 3) UIUX 무결성 — 잘림·끊김·오버플로 방지

- 모든 페이지가 360px (모바일) ~ 1920px (와이드) 범위에서 가로 스크롤 없음.
  `overflow-x: hidden` 을 body 또는 wrapper에 적용 가능.
- 토글·고정 버튼·진행바·커스텀 커서는 서로 또는 콘텐츠와 겹치지 않도록 z-index 명시:
  progress bar(100) > theme toggle(90) > custom cursor(9999, pointer-events:none) > content.
- 이미지 로드 실패에도 레이아웃 깨지지 않도록 `aspect-ratio` + 배경 placeholder 사용.
- 모든 클릭 가능한 요소는 최소 44×44px touch target.
- 메인 갤러리(루트 index.html) → 디자인 메인 → 상세 페이지 → 다시 갤러리로 돌아갈 수 있는 링크가 모든 페이지에 반드시 존재.
- 상세 페이지의 "← 목록으로" 또는 동등한 링크는 단순히 디자인 메인이 아닌 루트 `../../index.html` 로 돌아가는 옵션도 함께 제공 (작은 보조 링크로).
- 모달·툴팁·드롭다운이 viewport 밖으로 나가지 않도록 boundary 검사.
- 텍스트가 카드/박스를 벗어나서 잘리지 않도록 `word-break: keep-all` 또는 `overflow-wrap: anywhere` 사용 (한글 문장 단위 줄바꿈).

### 4) 카테고리 분포 v3 — 새 디자인 5종 추가

| 카테고리 | 디자인 번호 |
|---|---|
| 미니멀 · 뉴트럴 | #01 ~ #04, **#24 (Sketch)** |
| 에디토리얼 매거진 | #05 ~ #08, **#25 (Life Photo)** |
| 다크 · 테크 | #09 ~ #11 |
| 브루탈리스트 · 실험 | #12 ~ #14, **#21 (Risograph)** |
| 컬러풀 · 플레이풀 | #15 ~ #16, **#22 (Sticky Workspace)** |
| 럭셔리 · 세리프 시그니처 | #17 ~ #18 |
| 모션 · 인터랙션 강조 | #19 ~ #20, **#23 (Y2K Chrome)** |

---

# CATEGORY 8 (확장) — 추가 디자인 #21~#25

## DESIGN #21 — Risograph Duotone Print

- **무드**: 인디 인쇄 · 텍스처 · 따뜻함
- **추천 페르소나**: 디자이너 · 인쇄 · 인디 출판 · 일러스트 기획
- **레퍼런스**: rissotto.com, peoplemap.fr, perimetereditions.com
- **핵심**: 듀오톤 2색 잉크, 그레인 노이즈, 미스레지스터 ghost, 손글씨 액센트

```text
Build a Risograph print-style single-page portfolio.
Aesthetic: indie risograph print — duotone, grain texture, slight misregistration ghost shadows.

Palette PRIMARY: paper #F4EFE3 (offwhite paper), ink-1 #FF3366 (fluoro pink), 
ink-2 #2D3F6F (deep indigo), text #1A1A1A.

Typography: 'Fraunces' for headings (weight 600), 'IBM Plex Sans' for body 16px / 
line-height 1.65, 'Caveat' for handwritten accents.

Layout: max-width 960px, asymmetric 2-column. Image duotone via CSS filter 
+ blend mode (grayscale → mix-blend-mode multiply with ink-1, second layer with ink-2 
offset 4px → ghost / misregistration).
Add SVG grain noise overlay (5% opacity) site-wide.
Section dividers as hand-drawn squiggly lines (SVG).
Project cards slightly rotated (-1deg, +1deg) like loose printed pages.

Motion: paper shake-in on load. Image grain texture subtle pulse. Hand-drawn 
section underlines draw via stroke-dasharray.

Insert content: [HERO_COPY] [PROFILE_TEXT] [PROJECT_TEXTS] [CONTACT_INFO]
```

---

## DESIGN #22 — Sticky Note Workspace

- **무드**: 친근 · 협업 · PM 도구
- **추천 페르소나**: PM · 디자이너 · 워크숍 퍼실리테이터 · 컨퍼런스 스피커
- **레퍼런스**: miro.com, figma.com (FigJam), notion.so/templates
- **핵심**: 무한 캔버스 느낌, 포스트잇 4색, 핀·클립·실 연결선, 손글씨 폰트

```text
Build a Miro-board / sticky-note workspace style portfolio.
Aesthetic: PM workshop board — sticky notes, hand-drawn connection arrows, 
pinned cards, graph paper background.

Palette PRIMARY: bg #FAFAF7 (graph paper), text #1F2937, 
sticky colors: yellow #FFF59D, pink #F8BBD0, mint #C8E6C9, sky #B3E5FC.
Accent (pen ink) #1A1A1A. Connection arrows in #4B5563.

Typography: 'Patrick Hand' or 'Architects Daughter' for sticky text 18px, 
'Inter' for the underlying labels and meta 13px.

Layout: graph-paper background (CSS background pattern with 24px grid lines).
Sticky notes positioned absolutely with rotation -3deg ~ +3deg, hard offset shadow.
Hand-drawn SVG arrows connecting related notes.
A faux Kanban column row ("INBOX / DOING / SHIPPED") with project stickies.

Motion: stickies wobble on hover. Drag-to-rearrange (mouse + touch) on the 
"DOWNLOAD CV" sticker. Connection arrows draw-in on scroll.

Insert content: [HERO_COPY] [PROFILE_TEXT] [PROJECT_TEXTS] [CONTACT_INFO]
```

---

## DESIGN #23 — Y2K Chrome Glow

- **무드**: 메탈 · 홀로그래픽 · 사이버 럭셔리
- **추천 페르소나**: 게임 · 음악 · 패션 크리에이티브 · Z세대 브랜딩
- **레퍼런스**: heaven by marc jacobs, jpg.com, mschf.com
- **핵심**: 크롬 그라디언트 헤딩, iridescent 액센트, 별/하트 픽토그램, 디스코 글로우

```text
Build a Y2K chrome / iridescent portfolio.
Aesthetic: late-90s/early-00s metal chrome + holographic gradients — futuristic, 
playful, slightly retro.

Palette PRIMARY: bg #0F0F1A (deep purple-black), chrome white #F0F0FF, 
iridescent gradient (linear pink #FF66CC → cyan #66CCFF → mint #66FFCC), 
accent magenta #FF00FF.

Typography: 'Major Mono Display' or 'Orbitron' for headings with chrome 
gradient text fill (background-clip: text + iridescent gradient). 'Inter' for body 15px.
Hero name 96px with chrome gradient and a subtle text-shadow glow.

Layout: max-width 1100px. Glowing cards with rgba(255,255,255,0.08) bg + 
backdrop-filter blur + iridescent gradient borders (using border-image).
Star ★ and heart ♥ SVG decorations floating around hero.
Project cards shaped like chat bubbles or holographic price tags.

Motion: chrome gradient slowly rotates (shimmer animation 8s loop). 
Stars twinkle (scale + opacity stagger). Cards glow brighter on hover. 
Cursor leaves an iridescent trail.

Insert content: [HERO_COPY] [PROFILE_TEXT] [PROJECT_TEXTS] [CONTACT_INFO]
```

---

## DESIGN #24 — Hand-drawn Sketch Notebook

- **무드**: 미니멀 · 따뜻 · 손맛 · 노트북
- **추천 페르소나**: 디자이너 · 일러스트 · 기획 · 교육자
- **레퍼런스**: tldraw.com, excalidraw.com, are.na/feed
- **핵심**: 모눈 노트 배경, 손그림 SVG 박스·화살표, 핸드라이팅 폰트, 잉크 텍스처

```text
Build a hand-drawn notebook-style portfolio.
Aesthetic: sketched on a grid paper notebook — hand-drawn boxes, ink arrows, 
informal but considered.

Palette PRIMARY: bg #FAF8F2 (cream paper), grid #E5E0D5, text #2A2A2A, 
ink accent #1E3A8A (royal blue ink), highlight #FFE066 (yellow marker).

Typography: 'Caveat' or 'Architects Daughter' for handwritten headings 56px, 
'Inter' for body 16px / line-height 1.7.

Layout: graph-paper background (CSS repeating-linear-gradient for 24px grid).
Content sections wrapped in hand-drawn SVG boxes (slightly wobbly stroke).
Hand-drawn SVG arrows connecting sections.
Highlighter strokes behind key phrases (yellow marker effect).
Page corners with subtle "page fold" SVG shadows.

Motion: hand-drawn SVG boxes/arrows draw themselves on scroll using 
stroke-dasharray + stroke-dashoffset animation. Highlighter strokes "draw" 
horizontally across phrases. 

Insert content: [HERO_COPY] [PROFILE_TEXT] [PROJECT_TEXTS] [CONTACT_INFO]
```

---

## DESIGN #25 — LIFE Magazine Photo Spread

- **무드**: 클래식 사진 저널리즘 · 따뜻한 컬러 · 1970s LIFE
- **추천 페르소나**: 다큐멘터리 · 포토에세이 · 인터뷰어 · 큐레이터
- **레퍼런스**: life.com archive, magnum photos, california sunday magazine
- **핵심**: 풀블리드 사진 우선, 두꺼운 헤드라인, 손글씨 같은 캡션, 따뜻한 컬러 사진

```text
Build a 1970s LIFE magazine photo-spread style portfolio.
Aesthetic: classic photo journalism — full-bleed warm color photography, 
heavy serif headlines, italic caption strips.

Palette PRIMARY: bg #F2EDE3 (cream paper), text #1A1A1A, 
accent #8B0000 (LIFE red), muted #5A4F3F.

Typography: 'Old Standard TT' or 'Spectral' for headlines weight 700 / 64px, 
'Source Sans 3' for body 17px / line-height 1.7. Captions in 14px serif italic.

Layout: photo-first — each section starts with a full-bleed (or near-full) photo 
filling 70% of viewport, body text in narrow 480px column below or beside.
Magazine "spread" feel: page count "p. 042" in 11px serif italic top corner.
A heavy headline overlays the bottom of each lead photo (white on dark gradient).
Captions sit small italic beneath each photo with photographer credit style.

Motion: photo Ken Burns 1.1→1.0 on entry (2000ms slow). 
Headline text-split words slide up + fade on entry. 
A subtle page-turn transition between sections (translateY + opacity sequence).

Insert content: [HERO_COPY] [PROFILE_TEXT] [PROJECT_TEXTS] [CONTACT_INFO]
```

---

## 20종 분포

| 카테고리 | 디자인 번호 |
|---|---|
| 미니멀 · 뉴트럴 | #01 ~ #04 |
| 에디토리얼 매거진 | #05 ~ #08 |
| 다크 · 테크 | #09 ~ #11 |
| 브루탈리스트 · 실험 | #12 ~ #14 |
| 컬러풀 · 플레이풀 | #15 ~ #16 |
| 럭셔리 · 세리프 시그니처 | #17 ~ #18 |
| 모션 · 인터랙션 강조 | #19 ~ #20 |

---

# CATEGORY 1 — 미니멀 · 뉴트럴

## DESIGN #01 — Whitespace Cathedral

- **무드**: 차분 · 신뢰감 · 학구적
- **추천 페르소나**: 컨설팅 · 금융 · 연구직, 데이터를 차분히 정렬해서 보여주고 싶은 사람
- **레퍼런스**: linear.app, stripe.com/press
- **핵심**: 60% 이상의 여백, 좌측 정렬, 시리얼 넘버링, 단일 블루 액센트

```text
Build a single-page personal portfolio in plain HTML + Tailwind CSS + vanilla JS.
Aesthetic: Swiss minimalism with generous whitespace (60%+ of viewport),
strict 12-column grid, hairline #E5E5E5 dividers.

Palette: background #FAFAFA, body text #1A1A1A, muted #6B6B6B,
single accent #2563EB. No gradients. No shadows.

Typography: Inter for body, Inter Display for headings.
Hero name 96px / line-height 1.0 / tracking -0.02em / weight 500.
Section labels 12px uppercase mono, letter-spacing 0.15em.

Layout: everything left-aligned. Sections numbered as
"01 / Profile", "02 / Projects", "03 / Contact" in the label style.
Hero is name + one-sentence intro + blinking caret. No images.
Projects render as a vertical list (not a grid) with hairline dividers;
each row is "YEAR — TITLE — one-line outcome".
Hover: row background shifts to #F4F4F4 with 200ms ease.

Motion: 16px Y-translate fade-in on scroll via IntersectionObserver.
Mobile: single column, type scale stepped down two notches.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]  
```

---

## DESIGN #02 — Mono Serif Whisper

- **무드**: 사색적 · 정제됨 · 글 중심
- **추천 페르소나**: 에세이스트 · 기획자 · 라이터 지향, 사진보다 글의 밀도를 보여주는 사람
- **레퍼런스**: paulgraham.com, craigmod.com
- **핵심**: 중앙 정렬 단일 컬럼, 세리프 이탤릭 강조, 색 없이 흑백, 720px 본문 폭

```text
Build a single-page reading-first portfolio in pure HTML + Tailwind + vanilla JS.
Aesthetic: editorial monochrome — feels like a Substack longform essay.

Palette: background #FFFFFF, text #111111, muted #6B6B6B, rule #DDDDDD.
Use ZERO chromatic color. Italics carry all emphasis.

Typography: Tiempos Headline or Source Serif Pro for headings, weight 400.
Inter or Source Sans for body. Body 18px / line-height 1.7.
Hero name 64px serif italic, low contrast against background. 

Layout: single centered column, max-width 720px, ample top padding (160px).
No grid, no cards. Section breaks marked by centered hairline rule
flanked by a single asterisk character.
Projects render as essay-style paragraphs with bold project name as inline lead,
followed by italic role/year, followed by 2~3 line outcome paragraph.
A pull-quote appears between sections in 32px serif italic, left-rule treatment.

Motion: text fade-in only (no Y-translate). No hover effects beyond underline.
Mobile: same column, padding reduced.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #03 — Neutral Stone

- **무드**: 따뜻함 · 정돈됨 · 인간적
- **추천 페르소나**: HR · 교육 · 사회혁신 분야, 차갑지 않은 신뢰감을 주고 싶은 사람
- **레퍼런스**: mailerlite.com, framer.com, are.na
- **핵심**: 베이지 베이스 + 테라코타 액센트, 부드러운 그림자, 24px 라운드, 넉넉한 행간

```text
Build a single-page warm-neutral portfolio in plain HTML + Tailwind + vanilla JS.
Aesthetic: soft modern — like a well-designed independent magazine subscription page.

Palette: background #F5F1EB, surface #FFFFFF, text #2A2622,
muted #786E5F, accent #A88B6E terracotta.
Allow ONE soft drop shadow: 0 8px 24px rgba(42,38,34,0.06).

Typography: Söhne or GT Walsheim for headings, weight 500.
Inter for body 17px / line-height 1.65.
Hero name 80px / tracking -0.01em.

Layout: max-width 1080px, centered. Cards have 24px border-radius and 32px
internal padding. Hero spans full width with the name on the left
and a single circular avatar (200px) on the right.
Profile section is a two-column 60/40 split: bio left, soft list of values right.
Projects render as horizontal cards stacked vertically — image left (4:3),
text right; alternate sides every other card.

Motion: cards lift 4px on hover with shadow deepening; spring easing (220ms).
Mobile: cards stack to single column, images become 16:9 on top.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #04 — Bauhaus Grid

- **무드**: 구조적 · 단호함 · 모더니즘
- **추천 페르소나**: 디자이너 · 건축 · 산업디자인 지망, 시스템 사고를 강조하고 싶은 사람
- **레퍼런스**: bauhaus.de, swiss-style.org, dribbble Standalone studio profiles
- **핵심**: 6컬럼 가시 그리드, 빨강 단일 액센트, Helvetica, 모서리 시리얼 넘버

```text
Build a single-page modernist portfolio in plain HTML + Tailwind + vanilla JS.
Aesthetic: Swiss / Bauhaus — visible grid, primary geometric shapes, 
zero ornament.

Palette: background #F0F0F0, text #000000, accent #E63946 (single red).
Grid lines #DCDCDC, 1px hairline.

Typography: Helvetica Neue / Helvetica Now Display only.
Hero word-mark 120px black weight, all caps, tracking -0.03em.
Body 15px regular.

Layout: a 6-column grid VISIBLE as 1px guides in the background 
(toggleable by pressing 'G' in JS).
Hero takes columns 1-4. Profile takes columns 1-3.
Projects are placed in alternating asymmetric positions across the grid:
project 1 = columns 1-4, project 2 = columns 3-6, project 3 = columns 2-5.
A red filled square (24px) appears next to the active section heading.
Page corners show "P/01", "P/02"... section indicators in 10px mono.

Motion: grid guides briefly flash on page load.
Hover on project: red square color-fills the project number area.
Mobile: grid collapses to 2 columns; corner markers persist.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

# CATEGORY 2 — 에디토리얼 매거진

## DESIGN #05 — NYT Magazine Split

- **무드**: 권위 있는 · 저널리스틱 · 깊이 있는
- **추천 페르소나**: 저널리즘 · PR · 정책연구, 긴 호흡의 본문을 자신 있게 보여주는 사람
- **레퍼런스**: nytimes.com/magazine, theatlantic.com, ft.com
- **핵심**: 좌 텍스트 우 부유 이미지, 드롭캡, 중간 pull-quote, 신문체 컬러

```text
Build a single-page NYT-Magazine style portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: serious editorial longform with magazine flow.

Palette: background #FCFCFC, body text #0E0E0E, muted #555,
hairline #C9C5BD, accent #C8102E (NYT red).

Typography: Spectral or Source Serif Pro for headings (weight 600 italic for hero name),
Source Sans Pro 17px for body / line-height 1.7.
First letter of each section uses 96px drop-cap, serif, slight overhang.

Layout: two-column 65/35 split — left column carries the editorial body, 
right column carries floating "sidebar" elements (small portrait image, 
factbox in 13px sans, vertical date stamp).
Hero spans full width with the name as italic serif headline 88px,
deck (subtitle) below in 22px serif regular.
Pull-quotes appear every other section in 28px serif italic with
left rule in accent red.
Projects render as essay sections with title in red small-caps tracker,
followed by drop-cap body, followed by tag list in 11px mono.

Motion: only underline-grow on hover. No scroll animations.
Mobile: columns stack; sidebar elements inline between paragraphs.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #06 — Vogue Italia Spread

- **무드**: 우아함 · 도발적 · 럭셔리 패션
- **추천 페르소나**: 패션 · 뷰티 · 럭셔리 브랜드 · 큐레이터, 자신감 있는 비주얼이 있는 사람
- **레퍼런스**: vogue.it, ssense.com, theline.com
- **핵심**: 거대한 이탤릭 세리프, 비대칭 2단, 골드 액센트, 측면 수직 라벨

```text
Build a single-page high-fashion editorial portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: Italian Vogue spread — confident, sparse, dramatic.

Palette: background #FAF7F2 (warm cream), text #1A1614,
accent gold #8B6914, secondary accent #C9A961.

Typography: Playfair Display or Bodoni Moda for headings,
EB Garamond for body 16px / line-height 1.55.
Hero treatment: client name as 180px italic display weight 400,
ligatures enabled, sits low against generous whitespace.

Layout: asymmetric two-column where columns shift offset by 80px vertically.
Left margin always carries a vertical text label (writing-mode: vertical-rl)
naming the section in 11px mono uppercase gold.
Hero is a single line that BREAKS dramatically — first/last name on separate lines
with massive negative leading.
Projects render as full-bleed image (if provided) with caption 
overlapping bottom-left in italic, project name in gold small caps.
A thin gold horizontal rule (1px, 80px wide) separates sections.

Motion: hero name does letter-by-letter italic reveal (stagger 60ms) on load.
Hover on project image: image desaturates and overlay caption expands.
Mobile: vertical labels move to inline section openers.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #07 — Brutalist Newspaper

- **무드**: 분석적 · 정보 밀도 높음 · 인디 출판물
- **추천 페르소나**: 리서처 · 데이터 분석가 · 비평가, 정보량 자체를 강점으로 보여주는 사람
- **레퍼런스**: are.na/blog, dirt.fyi, blackbird.spy
- **핵심**: 신문지 톤 + 다단 본문 + 마스트헤드 헤더 + 인쇄용 점선/실선 분할

```text
Build a single-page newspaper-style portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: indie print broadsheet — dense, structured, slightly raw.

Palette: background #F4F1E8 (newsprint), text #1A1A1A,
accent #B91C1C (ink red), rule #2A2A2A.

Typography: IBM Plex Serif for headlines, IBM Plex Sans for body 14px /
line-height 1.55, IBM Plex Mono for labels and metadata.
Use real newspaper hierarchy: kicker (10px mono red caps) →
headline (52px serif bold) → deck (18px serif italic) → byline.

Layout: full-width masthead row at top — left has [USER NAME] in 36px serif
black bold, right has "Vol. 01 / Issue 2025" + current date in 11px mono.
A double horizontal rule (2px above 1px) sits beneath the masthead.
Body uses CSS multi-column layout (column-count: 3) for the profile section
with column-rule of 1px solid #2A2A2A.
Projects render as standalone "articles" each with kicker + headline +
byline + 3-column body. Image is treated as a captioned plate
with "FIG. 01" label.

Motion: NONE. Stillness is the point.
Hover on links: red underline appears.
Mobile: columns reduce to 1; masthead stacks; everything else preserved.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #08 — Whisper Editorial with Imagery

- **무드**: 시적 · 사진 중심 · 갤러리스트
- **추천 페르소나**: 사진 · 영상 · 비주얼 디자이너, 좋은 이미지 자료가 충분히 있는 사람
- **레퍼런스**: cereal-magazine.com, kinfolk.com
- **핵심**: 풀-블리드 흑백 이미지, 좌우 교차 레이아웃, 색은 호버 시 복귀

```text
Build a single-page photography-led editorial portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: Kinfolk / Cereal magazine — quiet, image-led, breathing room.

Palette: background #FFFFFF, text #1F1F1F, muted #888,
accent #000000 (used only as 1px hairlines).
Images shown as grayscale by default; hover returns to color.

Typography: DM Serif Display for hero name 96px regular,
Inter 16px for body / line-height 1.7.
Section titles 13px uppercase tracker-widened.

Layout: alternating modules — odd projects place image LEFT (60% width)
and text RIGHT; even projects flip. Modules separated by 160px vertical gap.
Hero is a single 16:9 full-bleed image (if provided; otherwise large blank
canvas with name overlaid bottom-left).
Each project caption uses a quiet metadata row: 
"Year — Role — Client" in 12px mono.
Profile section sits in centered 640px column, image as a 4:5 portrait
inserted as a float pulling text around (CSS shape-outside).

Motion: image grayscale → color on hover with 400ms ease.
Scroll triggers a 24px Y-fade on image modules.
Mobile: alternating layout collapses to image-top / text-below.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

# CATEGORY 3 — 다크 · 테크

## DESIGN #09 — Linear Dark Tech

- **무드**: 정밀 · 미래적 · 엔지니어링
- **추천 페르소나**: 개발자 · PM · 데이터엔지니어, 기술적 신뢰감을 주고 싶은 사람
- **레퍼런스**: linear.app, vercel.com, raycast.com
- **핵심**: 짙은 배경 + 미세 그라디언트 메시, 글로우 보더, 모노스페이스 메타데이터

```text
Build a single-page dark-mode tech portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: Linear / Raycast — precise, slightly glowing, engineering-grade.

Palette: background #0A0A0A, surface #141414, text #E5E5E5, muted #888,
gradient accent linear-gradient(135deg, #8B5CF6 0%, #06B6D4 100%).
Subtle radial gradient mesh in background (10% opacity).

Typography: Inter for body, JetBrains Mono for code/labels.
Hero name 80px / weight 600 / gradient text fill using accent gradient.
Body 15px / line-height 1.6 / color #E5E5E5.

Layout: max-width 1100px, centered. Hero has a vertical fade gradient overlay.
Sections divided by faintly glowing horizontal lines (1px, #8B5CF6 with 30% opacity).
Projects render as cards (background #141414, 1px border #2A2A2A) that on hover
gain a soft glow border in the gradient accent (box-shadow: 0 0 24px rgba(139,92,246,0.3)).
Each project card has a top-right monospace tag "v1.0 · YEAR".
A subtle command-palette-style search bar lives in the top-right (decorative).

Motion: cursor-following spotlight glow (radial gradient 400px) follows mouse,
multiplied with surface.
Card hover: scale 1.02, glow border, 200ms ease.
Mobile: spotlight disabled; cards stack with persistent glow.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #10 — Cyber Terminal

- **무드**: 너드 · 해커 · 레트로 컴퓨팅
- **추천 페르소나**: 보안 · 백엔드 · CS 동아리 · 게임 개발, 캐릭터를 강하게 박는 사람
- **레퍼런스**: hackernoon profiles, terminal.shop, blot.im
- **핵심**: 진검정 + 형광 그린 + 마젠타, VT323 폰트, ASCII 헤더, 깜빡이는 커서

```text
Build a single-page CRT-terminal style portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: 1980s green-phosphor terminal — fully simulated text UI.

Palette: background #000000, primary text #00FF94 (phosphor green),
accent #FF00C8 (magenta), warning #FFB000 (amber).
Add scanline overlay (1px horizontal stripes at 3% opacity).

Typography: VT323 for headings (large), JetBrains Mono 14px for body.
Cursor block (8x16px green) always blinks at end of current focus line.

Layout: full screen treated as a single tty.
Top status bar: "guest@portfolio:~$ " prompt, current path, time in 12px mono.
Hero is an ASCII-art name banner (generated big-block letters) followed
by a typewriter-effect intro line typing one character at a time on load.
Sections shown as command outputs:
  "> cat profile.txt" → profile body in green
  "> ls projects/" → list of projects with timestamps and sizes
  "> ./contact" → contact info appearing line by line
Project hover triggers a fake "loading..." spinner (rotating | / - \) for 400ms.

Motion: typewriter reveal on first load; cursor blink (530ms cycle);
fake commands echo on link clicks.
Mobile: ASCII banner shrinks; everything else preserved.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #11 — Vercel Pro Spotlight

- **무드**: 미니멀 다크 · 프리미엄 · 차분한 자신감
- **추천 페르소나**: 시니어 개발자 · 테크리드 지망, 글로우는 줄이고 클래스를 보여주는 사람
- **레퍼런스**: vercel.com, cal.com, resend.com
- **핵심**: 진검정 + 백색 + 미세 회색, 1px 헤어라인 보더, 마우스 스포트라이트, 글래스 카드

```text
Build a single-page premium-dark portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: Vercel landing page — restrained, premium, monochrome with a single 
ambient gradient orb.

Palette: background #000000, surface #0A0A0A, text #FFFFFF, muted #A1A1AA,
hairline #27272A. ONE ambient radial-gradient orb top-right
(rgba(120,119,198,0.15) → transparent).

Typography: Geist Sans for body, Geist Mono for labels and code.
Hero name 72px / weight 600 / tracking -0.04em.
Body 15px / line-height 1.6.

Layout: max-width 1200px. Hero is centered with name + one-line role
+ pill-shaped status indicator (green dot + "Available for work") in muted style.
Sections separated by 1px #27272A hairlines.
Projects render as cards with:
  - 1px #27272A border, 12px radius, 24px padding
  - background blur(8px) over the background gradient (glassmorphic feel)
  - top-right: arrow icon that translates 4px on hover
  - bottom-left: small monospace "01 · 02 · 03" indicator
A custom cursor-following spotlight (radial 600px, opacity 8%) brightens
cards as the cursor passes.

Motion: spotlight follows cursor with 120ms ease-out delay.
Card hover: border brightens to #3F3F46, arrow translates.
Mobile: spotlight disabled; arrow icons persist.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

# CATEGORY 4 — 브루탈리스트 · 실험

## DESIGN #12 — Web Brutalism Raw

- **무드**: 거칠음 · 무신경 · 90년대 GeoCities
- **추천 페르소나**: 인디 크리에이터 · 실험적 아티스트, 일부러 안 다듬은 인상을 노리는 사람
- **레퍼런스**: brutalistwebsites.com, mschf.com, kanyewest.com
- **핵심**: 형광 노랑 + 검정 4px 보더, Times + Arial 강제 클래시, 의도적 정렬 실패

```text
Build a single-page web-brutalism portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: raw early-web brutalism — intentionally ugly, no easing, no polish.

Palette: background #FFFF00 (full fluoro yellow), text #000000,
border #000000 (always 4px solid). Splash color #FF0000 used sparingly.

Typography: Times New Roman for headings (italic when emphasized),
Arial 16px for body. NO custom fonts. NO font smoothing.
Hero name 100px Times bold, slightly clipped at top.

Layout: NO grid. Elements placed with absolute positioning
in deliberately misaligned spots (e.g., profile box rotated -2deg).
Every element gets a 4px black border AND a 6px hard offset shadow (#000).
Sections are unstyled <div> boxes with the section name as the <h2>
underlined twice with a thick black line.
Projects render as boxes with hand-typed-looking metadata
(e.g., "made: 2024 / type: project / status: done").
Include a fake "under construction" gif placeholder (yellow caution stripes).

Motion: NONE. No transitions, no animations. Hover changes:
links go from black to red instantly (no ease).
Mobile: same chaos preserved — overflow allowed.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #13 — Anti-Design Collage

- **무드**: 어수선함 · 펑크 · 콜라주
- **추천 페르소나**: 그래픽 아티스트 · 인디 디자이너 · Z세대 크리에이티브
- **레퍼런스**: studiofeixen.ch, dazeddigital.com, eddesignmuseum.com
- **핵심**: 여러 색 블록 겹침, 스티커형 요소, 의도적 폰트 클래시, 드래그 가능 오브제

```text
Build a single-page anti-design collage portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: pasted-zine collage — overlapping color blocks, sticker-like elements,
deliberate font clash.

Palette: simultaneous blocks of #FF0099 (magenta), #00FFCC (mint),
#FFFF00 (yellow), #000000 (text/borders).
No background base color — the whole page is a composition of colored shapes.

Typography: Helvetica Black for one third of text, Comic Sans for another third,
Times Italic for the rest. Sizes wildly varying (12px next to 96px).
Hero name uses three different fonts across letters (deliberate clash).

Layout: NO grid. Color blocks (rectangles, circles, irregular SVG blobs) 
are scattered as positioned absolute. Text sits on top, often overlapping 
the edges of the blocks. Project sections rendered as "sticker" cards 
rotated random angles (-5deg to +5deg) with hard offset shadows.
Hand-drawn arrow SVGs point to certain project highlights.
A draggable "DOWNLOAD CV" sticker lives somewhere on the page 
(JS implements drag with mouse / touch).

Motion: stickers wobble slightly on hover (rotate ±1deg, 100ms).
Background colors swap when a project section enters viewport.
Mobile: chaos preserved; draggable sticker becomes tap-to-move.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #14 — Concrete Modernism

- **무드**: 묵직함 · 산업적 · 거대한 타이포
- **추천 페르소나**: 건축 · 산업디자인 · 모션 디자이너, 압도적 첫인상을 노리는 사람
- **레퍼런스**: locomotive.ca, active-theory.com, build-in-amsterdam.com
- **핵심**: 다크 콘크리트 + 세이프티 오렌지, Druk 컨덴스드, 직각 모서리, 큰 블록

```text
Build a single-page heavy-modernist portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: Locomotive-studio energy — concrete textures, massive condensed type,
zero rounded corners.

Palette: background #2A2A28 (warm dark concrete), text #F2F2F0 (chalk),
accent #FF5800 (safety orange), secondary #9C9C99 (steel).
Add subtle noise/grain overlay (5% opacity).

Typography: Druk Wide Bold (or "Anton" as free fallback) for hero — 
ALL CAPS, 160px, line-height 0.85, letter-spacing -0.04em.
Söhne Mono 13px for metadata. NO serifs anywhere.

Layout: full-bleed full-height hero with the name STACKED vertically 
(one word per line) filling the screen, anchored bottom-left.
Sections divided by floor-to-ceiling vertical orange lines (4px solid #FF5800).
Profile sits in a left-aligned narrow column (480px max).
Projects shown as large rectangular blocks (no padding from edges),
each project takes 60vh, image (if any) fills it, project name in 
massive condensed caps overlaid bottom.
Footer has a giant arrow ↗ pointing to the contact email.

Motion: hero name letters shift up 8px sequentially on load (stagger 50ms).
Project blocks transition image scale 1.0 → 1.05 on enter.
Cursor becomes a 32px orange filled circle on link hover.
Mobile: vertical lines hidden; hero stack compressed to 80px.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

# CATEGORY 5 — 컬러풀 · 플레이풀

## DESIGN #15 — Notion-Pop Bright

- **무드**: 친근함 · 정리되어 있음 · 협업적
- **추천 페르소나**: PM · 마케터 · 교육 · 컨퍼런스 스피커, 따뜻하고 똑똑해 보이고 싶은 사람
- **레퍼런스**: notion.so, linear.app/method, superhuman.com
- **핵심**: 화이트 베이스 + 5가지 파스텔 카테고리 컬러, 16px 라운드 카드, 이모지 친화

```text
Build a single-page friendly-bright portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: Notion docs landing — clean, smart, approachable, with categorical
pastel accents.

Palette: background #FFFFFF, text #1F2937, muted #6B7280.
Five pastel category colors (each tied to a project tag):
#FEE2E2 (red-tint), #FEF3C7 (amber-tint), #D1FAE5 (mint-tint), 
#DBEAFE (sky-tint), #EDE9FE (lavender-tint).
Borders are 1px in slightly darker variants of each tint.

Typography: DM Sans for body 16px / line-height 1.6, 
DM Serif Display for hero name 56px.
Section titles 22px DM Sans semibold.

Layout: max-width 960px centered. Hero is two-column: left has name + 
intro + a single chosen emoji at 64px floating top-right of the text block.
Profile uses a "card with rounded corners and 1px border" pattern.
Projects render as horizontal cards, each card tinted with one of the
five pastels, with a 32px emoji as a category icon top-left.
Each card has a "tag pill" row in bottom showing project tags.

Motion: cards lift 2px on hover with subtle shadow (200ms spring).
Emoji rotates 8deg on hover.
Page-load cards fade in with stagger 80ms.
Mobile: cards stack; emojis remain prominent.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #16 — Gradient Mesh Joy

- **무드**: 활기참 · 미래낙관 · 컬러풀
- **추천 페르소나**: 비주얼 디자이너 · 모션 · 광고 크리에이티브, 컬러 자체로 기억되고 싶은 사람
- **레퍼런스**: stripe.com (older), framer.com, mailchimp.com (newer)
- **핵심**: 움직이는 그라디언트 메시 배경, 글래스 카드, 다채로운 커서 트레일

```text
Build a single-page gradient-mesh portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: optimistic future — animated mesh gradient background, glassmorphism, 
joyful color.

Palette: background as animated mesh of #FF6B9D (pink), #FFA45B (peach),
#7B61FF (purple), #4ECDC4 (teal). Animation rotates these hues slowly (30s cycle).
Surface (cards): rgba(255,255,255,0.12) with backdrop-filter: blur(20px).
Text #FFFFFF semi-bold, muted rgba(255,255,255,0.7).

Typography: Cabinet Grotesk or Satoshi for body 16px / line-height 1.6.
Hero name 88px / weight 700 / letter-spacing -0.03em.
Headings 28px weight 600.

Layout: max-width 1100px centered. Background SVG mesh covers viewport.
Hero is centered with the name on top of the mesh, glass card holding
the intro text below.
Profile sits inside a single large glass card (40px padding, 24px radius).
Projects render as a 3-column glass card grid (1-column on mobile).
Each project card has a single emoji at 48px as visual anchor.
Footer is a final glass card with the contact info.

Motion: mesh gradient animates 30s loop using CSS animated background-position.
Cursor leaves a trail of 6 small colored dots that fade out (60ms each).
Cards lift 8px and intensify blur on hover.
Mobile: trail disabled; mesh animation slowed to 60s.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

# CATEGORY 6 — 럭셔리 · 세리프 시그니처

## DESIGN #17 — Maison Couture

- **무드**: 럭셔리 · 미니멀 · 시크
- **추천 페르소나**: 럭셔리 · 패션 · 호스피탈리티 · 프리미엄 브랜딩 지망
- **레퍼런스**: dior.com, hermes.com, aesop.com
- **핵심**: 진검정 베이스 + 단일 골드 액센트, 얇은 보도니, 수직 워드마크, 글자별 등장

```text
Build a single-page luxury-fashion portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: Maison couture — black, restrained, one shimmer of gold.

Palette: background #0F0E0C (warm black), text #EBE3D5 (champagne),
single accent #C9A961 (muted gold). NO other color.

Typography: Bodoni Moda for hero name 140px thin (weight 200) italic,
sized to dominate the viewport.
Inter 15px for body / line-height 1.7 / color #EBE3D5 with 80% opacity.
Section markers 11px serif small caps tracker-widened in gold.

Layout: viewport-anchored. Hero name centered horizontally with massive
top/bottom margin (240px each).
Top-left fixed: vertical word-mark (one's name) in 12px gold caps,
writing-mode vertical-rl.
Top-right fixed: tiny "EST. 2025" or "AT., R.O.K." label in gold.
Profile sits in a 520px narrow center column, body justified.
Projects render as full-width image (if any) with caption in italics
floating bottom-right; image gently darkens with overlay gradient.
Sections separated by a 1px gold rule (60px wide, centered).

Motion: hero name reveals letter-by-letter italic fade (stagger 80ms) on load.
Cursor becomes a small gold dot (8px) inside a 32px outlined ring on link hover.
Project image overlay deepens on hover (300ms).
Mobile: hero name scaled to 80px; vertical word-mark moves to top inline.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #18 — Heritage Auction

- **무드**: 권위 · 역사성 · 클래식
- **추천 페르소나**: 법조 · 회계 · 외교 · 미술품 · 클래식 음악, 보수적 무게감을 노리는 사람
- **레퍼런스**: sothebys.com, christies.com, gagosian.com
- **핵심**: 크림 베이스 + 버건디 액센트, 클래식 세리프, 페이지 넘김형 트랜지션, 액자 이미지

```text
Build a single-page heritage / auction-house style portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: Sotheby's calm authority — cream paper, burgundy detail, classical proportion.

Palette: background #F5EFE5 (cream paper), text #1A1A1A,
accent #4A2C2A (deep burgundy), secondary #8B7355 (antique gold).
Hairline rules in #4A2C2A.

Typography: Canela or Lyon Display for headings (weight 500), 
EB Garamond for body 17px / line-height 1.65.
Hero name 84px serif regular, italic for last name.
Section titles 14px serif small caps with letter-spacing 0.18em.

Layout: 1080px max centered, generous side margins.
Hero is symmetric: centered name, centered single-line role,
flanked by ornamental hairline rules with small decorative bullets (•).
Profile in a centered 640px column.
Projects render as "lot entries":
  - Lot number in burgundy small caps top-left (LOT 01)
  - Project image as a framed plate (8px cream border + 1px burgundy outline)
  - Title in serif italic, year + role in small caps, 
    estimate-style "OUTCOME · IMPACT" line
  - Description body paragraph
Footer has a symmetric crest-like layout with contact info beneath an ornamental rule.

Motion: page sections cross-fade as if turning paper pages 
(opacity + 8px Y-translate, 600ms).
Hover on lot: framed image subtly brightens.
Mobile: lot layout stacks; framing preserved.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

# CATEGORY 7 — 모션 · 인터랙션 강조

## DESIGN #19 — Scroll Cinema

- **무드**: 영화적 · 몰입형 · 스토리텔링
- **추천 페르소나**: 영상 디렉터 · 모션 디자이너 · 인터랙티브 아티스트, 시간축 스토리가 있는 사람
- **레퍼런스**: bruno-simon.com (스크롤 챕터판), apple.com (제품 페이지), zara.com
- **핵심**: 가로 스크롤 프로젝트, 패럴랙스 레이어, 스티키 헤로, 스크롤마다 배경 변화

```text
Build a single-page scroll-driven cinematic portfolio in HTML + Tailwind + vanilla JS.
Aesthetic: Apple product page — sticky scenes, parallax layers, 
each scroll segment is a "scene".

Palette: shifts per scroll chapter:
  Chapter 1 (Hero):    bg #0A0A0A / text #FFF
  Chapter 2 (Profile): bg #E5E2DC / text #1A1A1A
  Chapter 3 (Project1):bg #1F2937 / text #FFF
  Chapter 4 (Project2):bg #FEF3C7 / text #1F2937
  Chapter 5 (Project3):bg #064E3B / text #FFF
  Chapter 6 (Contact): bg #FFFFFF / text #000
Transitions between chapters fade backgrounds and text colors.

Typography: Migra or Tobias for hero serif 120px weight 300,
Inter 16px for body. Each chapter heading 64px serif.

Layout: each "chapter" is a 100vh sticky section.
Hero chapter: name centered with a slow parallax floating object behind 
(SVG circle 600px) shifting on scroll position.
Profile chapter: text appears with a 50px Y-translate fade as it enters.
Each project chapter: image (or large emoji as placeholder) on one side, 
text on the other, both at different parallax speeds (image 0.8x, text 1.0x).
A persistent left-side progress indicator (vertical line + dot) shows 
chapter progress.

Motion: heavy use of IntersectionObserver and scroll-linked transforms.
GSAP-equivalent simulated with requestAnimationFrame.
Cursor becomes a 64px outlined circle that scales 2x on links.
Mobile: parallax reduced to subtle Y-translate only; chapters preserved.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

## DESIGN #20 — 3D Object Showcase

- **무드**: 인터랙티브 · 첨단 · 장난기
- **추천 페르소나**: 인터랙티브 개발자 · 게임 · XR · 크리에이티브 코딩, 디지털 자체를 자랑할 사람
- **레퍼런스**: bruno-simon.com, lusion.co, active-theory.com
- **핵심**: 중앙 회전 3D 오브제, 오비탈 프로젝트 카드, 마우스 틸트 패럴랙스

```text
Build a single-page 3D-object-driven portfolio in HTML + Tailwind + vanilla JS.
Use Three.js (via CDN) for the central 3D object — fallback to CSS 3D 
if Three is unavailable.
Aesthetic: playful technical showcase — one hero 3D object as the page's anchor.

Palette: background #ECECEC (soft stage), text #111111,
single accent #3B82F6 (electric blue) used on hover states only.

Typography: Beatrice Display or Inter Display for hero 72px weight 700,
Inter 15px for body. Project labels 12px monospace.

Layout: viewport-centered with a 3D object (icosahedron or torus, 320px) 
sitting in the upper center. The object slowly rotates on its own (0.005 rad/frame)
and reacts to mouse position (subtle tilt: ±15deg pitch/yaw).
Hero name sits BELOW the 3D object, in 72px display.
Profile and project sections appear as "orbital" cards arranged around 
the page below the hero — each card is a 320px rounded rectangle that 
on hover lifts 12px with a soft shadow.
A small "Click to interact" hint appears beneath the 3D object on first load.
Clicking the object snaps to a wireframe view (toggle).

Motion: 3D object rotation + mouse-driven tilt with damping.
Cards have a 3D tilt effect on hover (mousemove → rotateX / rotateY ±8deg).
Cursor leaves no trail; cursor becomes blue dot on interactive elements.
Mobile: 3D object simplified to a 2D rotating SVG; tilt effects disabled.

Insert content:
[HERO_COPY]
[PROFILE_TEXT]
[PROJECT_TEXTS]
[CONTACT_INFO]
```

---

# 부록 — 선택 가이드 (브랜딩 명세서 매칭 매트릭스)

브랜딩 명세서의 '선호 무드'와 '선호 컬러 톤' 두 축으로 매칭하세요.
한 셀에 여러 디자인이 있으면 '추천 페르소나'를 추가 기준으로 사용합니다.

| 무드 \ 컬러 톤 | 모노크롬 | 뉴트럴 | 비비드 / 컬러풀 | 다크 | 럭셔리 (블랙+골드/버건디) |
|---|---|---|---|---|---|
| 차분 · 신뢰감 | #01, #02 | #03 | — | #11 | #18 |
| 구조적 · 시스템 | #04 | — | — | #09 | — |
| 저널리스틱 · 정보 밀도 | #02, #07 | #05 | — | — | — |
| 시적 · 비주얼 | — | #08 | — | — | #17 |
| 너드 · 해커 | — | — | — | #09, #10, #11 | — |
| 거칠음 · 실험 | — | — | #12, #13 | — | — |
| 산업적 · 강한 첫인상 | #04 | — | — | #14 | — |
| 친근 · 협업적 | — | #15 | #15 | — | — |
| 활기참 · 미래낙관 | — | — | #16 | — | — |
| 럭셔리 · 클래식 | — | — | — | #17 | #17, #18 |
| 영화적 · 몰입형 | — | — | #19 | #19 | — |
| 인터랙티브 · 첨단 | — | #20 | — | — | — |

# 부록 — 빠른 의사결정 3 질문

1. **이미지 자료가 있는가?**
   - 있음 → #03, #06, #08, #18 우선
   - 없음 → #01, #02, #07, #09, #11, #17 (텍스트 중심)
2. **첫인상을 '무게감'으로 잡을지 '친근감'으로 잡을지?**
   - 무게감 → #14, #17, #18
   - 친근감 → #03, #15, #16
3. **개발자 / 기술직 신호가 필요한가?**
   - 필요 → #09, #10, #11, #20
   - 불필요 → 나머지

---

# 사용 시 주의

- 본인 보유 자료(이미지·로고)가 없는데 이미지 비중이 큰 디자인(#06, #08, #18, #20)을
  고르면 v0가 placeholder 이미지를 채워주지만 최종 폴리싱 시 빈 자리가 도드라집니다.
- 인터랙션 강한 디자인(#13, #16, #19, #20)은 모바일 환경에서 동작이 무거울 수 있으므로
  Part 2 STEP 4 모바일 최적화 단계에서 모션을 줄이는 후속 프롬프트가 필요합니다.
- 디자인을 골랐다면 해당 프롬프트를 Gemini AI Studio에 그대로 던지되, 첫 응답이 마음에 들지 않으면
  "Refine the [section name] — keep the palette and typography intact, but [요구사항]"
  형식으로 부분 리파인 프롬프트를 던지세요.
