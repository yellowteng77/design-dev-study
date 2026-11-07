# 1강 02장. 토큰 — 코드와 Figma를 양방향으로 잇기

**화면 변화**: Figma `DS` 파일에 `Primitives`·`Semantic` 변수 컬렉션과 Text·Effect 스타일이 생겼다. Figma에서 바꾼 색이 데모 화면의 버튼 색까지 왔다.

## 네 단계

| 단계 | 한 것 | Figma로 치면 |
|---|---|---|
| 1 | `tokens.json` 읽기 | 팔레트·시맨틱 변수, Number 변수, Text·Effect 스타일을 적어 둔 원본 |
| 2 | `npm run tokens` | 라이브러리 **게시** (JSON → `tokens.css`) |
| 3 | 플러그인 `ds Token Importer` 실행 | 코드 값으로 Variables를 자동 생성 (별칭·코드 구문까지) |
| 4 | Figma에서 `brand/600`을 보라로 → 코드로 가져오기 | 디자이너가 바꾼 값을 개발 원본에 받아 적기 |

## `tokens.json`의 네 묶음

| 묶음 | 예시 | Figma로 치면 |
|---|---|---|
| 원시 색 `color` | `brand.600 = #3b5bdb` | 팔레트 변수 |
| 의미 색 `semantic` | `primary = {color.brand.600}` | 시맨틱 변수 (팔레트를 별칭으로 가리킴) |
| 치수 `space`, `radius` | `space.4 = 16px` | Number 변수 |
| 글꼴·그림자 `font`, `shadow` | `font.label`, `shadow.card` | Text Style, Effect Style |

다크 테마(`themes/dark.json`)는 **의미 색만** 다른 팔레트 색으로 바꿔 가리킨다. = 시맨틱 컬렉션의 Dark 모드 칸.

## Figma에서 확인한 것

- 변수 창 왼쪽에 `Primitives`, `Semantic` 두 컬렉션
- `Semantic / primary`: Light = `color/brand/600`, Dark = `color/brand/300` (HEX가 아니라 별칭 칩)
- 변수 편집 → 코드 구문 → 웹 = `var(--ds-semantic-primary)` → Dev Mode에서 개발자가 보는 이름표

## 가장 중요한 규칙: 원본은 한 곳

코드와 Figma 양쪽에서 동시에 고치면 어느 값이 맞는지 알 수 없다. 팀에서 "원본은 `tokens.json`" 또는 "원본은 Figma" 중 하나로 정하고, 반대쪽은 받아 가기만 한다.

## 실수에서 배운 것

1. **변수 폴더를 헷갈렸다.** Figma 변수 창은 이름의 `/`를 폴더처럼 묶어 보여 준다. `brand/600`을 바꿔야 하는데 `red` 폴더에 600이 없어서 새 변수를 만들어 버렸다. → 기존 변수의 **값(색 칩)** 만 바꾼다. 새 변수를 만들면 코드와 어긋난다.
2. **원본을 고치고 게시를 안 했다.** `tokens.json`을 되돌렸는데 화면이 안 바뀌었다. 화면은 `npm run tokens`가 만든 `tokens.css`를 보기 때문이다.

| 코드 | Figma |
|---|---|
| `tokens.json` 수정 | 라이브러리에서 변수 값 수정 |
| `npm run tokens` | **게시** 버튼 |
| 브라우저 새로 고침 | 사용하는 파일에서 라이브러리 업데이트 받기 |

→ `tokens.js`, `tokens.css`는 자동으로 만들어지는 결과물이라 손대지 않는다. (00장에 이어 두 번째로 실수함)
