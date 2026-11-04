# 1강 01장. Figma MCP 연결 — Claude에게 Figma 파일 열쇠 주기

**화면 변화**: 코드 화면은 그대로다. 대신 Claude가 내 Figma 계정과 파일에 접속할 수 있게 됐고, 실습용 빈 파일 `DS`가 생겼다.

## 한 것

| 한 것 | Figma로 치면 |
|---|---|
| Figma MCP 연결 확인 | Claude에게 플러그인을 설치해 주고 파일 접근 권한을 준 것 |
| 플랜 제한 확인 | 무료 팀 말고 Pro 팀에 작업 파일 두기 |
| `CLAUDE.md` 훑어보기 | 팀 디자인 시스템의 네이밍 컨벤션 문서. Claude가 폴더를 열 때마다 자동으로 읽는다 |
| 실습 파일 `DS` 만들기 | 02장부터 Variables와 컴포넌트를 채울 빈 라이브러리 파일 (페이지: Foundations / Components / Playground) |

## 플랜 제한 — 파일을 어느 팀에 두느냐가 중요하다

- Claude가 Figma를 **읽는** 기능은 Starter(무료) 플랜 팀 파일에서 **한 달에 6번**만 쓸 수 있다.
- Claude가 Figma에 **그리는** 기능(변수·컴포넌트 만들기)은 이 제한과 따로다.
- 그래서 실습 파일은 Pro 플랜 팀(`K3i_UIUX Design Team`)에 만들었다.

## 이름표 맞추기 — 같은 값, 세 곳의 이름

| 원본 (`tokens.json`) | Figma Variables | 코드 (CSS) |
|---|---|---|
| `color.brand.600` | `Primitives` / `color/brand/600` | `--ds-color-brand-600` |
| `semantic.primary` | `Semantic` / `primary` (Light·Dark 모드) | `--ds-semantic-primary` |

점(`.`)이 Figma에서는 슬래시(`/`), 코드에서는 하이픈(`-`)으로 바뀔 뿐 같은 값이다.

## 기억할 규칙 세 가지

1. 시맨틱 색은 **원시 색의 별칭**으로 만든다. HEX 직접 입력 금지.
2. 컴포넌트 Variant 이름은 코드와 **글자 하나까지** 같게 (`variant = primary`, `size = sm`, 대소문자까지).
3. 레이아웃은 **Auto Layout**, 간격·색은 **변수 바인딩**만. 숫자·HEX 직접 입력 금지.

→ 다 외울 필요는 없다. Claude가 이 규칙대로 만드니, 결과물을 이 표와 대조해서 검수하면 된다.
