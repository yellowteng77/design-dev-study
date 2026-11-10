# 1강 03장. Lit 컴포넌트 뜯어보기

**화면 변화**: 없음. 코드를 고치지 않고 읽기만 한 장. 대신 개발자 도구에서 `ds-button`의 `variant`를 `ghost`로 바꾸자 버튼 색이 바로 바뀌는 걸 확인했다.

## 입구: `src/index.ts`

라이브러리 **게시(Publish)** 역할. 이 파일을 불러오면 `<ds-button>`·`<ds-input>`·`<ds-card>`·`<ds-switch>` 태그를 쓸 수 있게 된다.

## ds-button — 속성이 곧 Variant

| 코드 | Figma로 치면 |
|---|---|
| 속성 `variant`, `size` | Variant 속성 Type, Size |
| 속성 `disabled`, `loading` | Boolean 속성 |
| `var(--ds-semantic-primary)` | Fill에 hex 대신 **Variable 연결** |
| `:host([variant="ghost"])` 스타일 묶음 | Component Set 안의 Type=Ghost 변형 하나 |
| `...:hover` | Hover 상태 (Interactive component) |
| `var(--ds-radius-md, 8px)`의 `8px` | 토큰을 못 불러왔을 때 쓰는 비상용 값 |
| 폼 연결 (`formAssociated`) | 잠긴 그룹 안 버튼에 "클릭 → 폼 제출" 인터랙션을 직접 연결 |
| `part="base"`, `--ds-button-min-width` | 인스턴스에서 **허용한 오버라이드**만 열어 둔 것 |
| 슬롯 `icon-start`, `icon-end` | 아이콘 Instance swap 자리 |

## ds-input — 한 몸으로 묶은 Text field

- **라벨·입력칸·도움말을 한 컴포넌트 안에** 둔다. 스크린 리더용 연결(이 라벨은 이 칸의 것)이 잠긴 그룹의 벽을 넘지 못하기 때문.
- 신호 두 개: `ds-input` = 글자 칠 때마다, `ds-change` = 입력을 마쳤을 때(칸 벗어남·Enter).
- 제출하면 스스로 검사 → 틀리면 `invalid`: 빨간 테두리 + **도움말 자리에 오류 문구가 대신** 들어감(레이아웃이 안 흔들림). 다시 치면 바로 꺼짐.
  = State=Default / State=Error 변형이 검사 결과에 따라 자동으로 바뀌는 것.

## ds-card — 틀은 지키고 내용은 슬롯으로

| 코드 | Figma로 치면 |
|---|---|
| 슬롯 `header`·본문·`footer` | 카드 안의 Slot 자리. 인스턴스마다 다른 내용을 끼움 |
| `<h2 slot="header">` | "이건 header 자리에 넣어"라는 이름표. 이름표 없으면 본문으로 |
| `elevated` 켜기 | Style=Outlined → Elevated (테두리 대신 그림자 토큰) |
| 빈 슬롯 자동 숨김 | 오토 레이아웃에서 레이어를 숨기면 간격까지 사라지는 것 |

## 공통 스타일: `src/styles/shared.ts`

- `baseStyles` = 기본 글꼴·글자색 → 파일 전체의 기본 Text style
- `focusRing` = Tab으로 이동할 때 생기는 테두리 → 모든 컴포넌트 공통 Focus 상태. 한 곳만 바꾸면 전부 바뀐다.

## 디자인을 지키는 세 가지 장치 (00장에서 본 현상의 이유)

| 장치 | 뜻 | Figma로 치면 |
|---|---|---|
| ① Shadow DOM 격리 | 앱의 전역 CSS가 안쪽에 못 닿음 | Detach 없이는 라이브러리 컴포넌트를 못 고침 |
| ② CSS 변수만 통과 | 벽을 넘는 건 토큰 값뿐. 테마는 토큰만 바꿈 | Variable Mode를 Light → Dark로 |
| ③ 공개 지점만 열기 | `::part`, 지정한 CSS 변수, 슬롯만 손댈 수 있음 | 허용된 속성만 오버라이드 |

## 따라 하기에서 확인한 것

1. 버튼 오른쪽 클릭 → 검사 → `#shadow-root (open)` 펼치기 → 안에 `<button part="base">`. = 인스턴스를 더블클릭해서 안쪽 레이어 보기.
2. Elements 패널의 `<ds-button variant="primary">`에서 `primary`를 더블클릭 → `ghost` → 색이 바로 바뀜. 코드는 한 줄도 안 고쳤다.
   - 교재에는 "Styles에서"라고 적혀 있지만 실제로 고치는 곳은 **Elements 패널의 태그 줄**.

**한 줄 요약**: Lit 컴포넌트는 Figma 라이브러리 컴포넌트와 같은 방식으로 지켜진다. 잠겨 있고, 색은 Variable로 정해지고, 허용된 곳만 바뀐다.
