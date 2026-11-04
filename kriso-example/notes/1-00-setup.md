# 1강 00장. 개요와 준비 — 데모 화면으로 디자인 시스템 체험

**화면 변화**: 재료를 내려받고(`npm install`) 데모를 띄우니(`npm run dev`) 브라우저에 `@k3i/ds — Lit 디자인 시스템` 화면이 나온다.

## 화면에서 확인한 다섯 가지

| 확인한 것 | 해 본 것 | Figma로 치면 |
|---|---|---|
| ① 버튼 종류·크기 | Primary·Secondary·Ghost·Danger, Small·Medium·Large, Disabled, 저장 중 | 메인 컴포넌트 하나(`<ds-button>`)에 Variant 속성만 바꾼 인스턴스들 |
| ② 다크 테마 | `다크 테마 전환` 버튼 → 화면 전체 색이 바뀜 | Variables의 Light/Dark 모드 전환. 컴포넌트는 그대로, 변수 값만 바뀐다 |
| ③ 전역 CSS 공격 | 엉망 스타일이 걸린 카드에서 일반 버튼만 망가지고 `ds-button`은 멀쩡 | 바깥에서 수정 못 하게 잠긴 컴포넌트. 이 보호막이 **Shadow DOM** |
| ④ 폼 검증 | 빈 칸으로 가입 → 빨간 오류, 제대로 입력 → 값 정리, 초기화 | 직접 만든 입력칸이 브라우저 기본 입력칸처럼 정식 부품 대접을 받는다 (**ElementInternals**) |
| ⑤ 토큰 바꾸기 | `brand.600`을 보라색으로 → Primary·가입 버튼이 보라색 | 원시 변수 한 칸을 바꾸면 별칭(alias)으로 연결된 곳이 모두 따라 바뀐다 |

## 토큰이 흐르는 길

```
tokens/tokens.json  →  (npm run tokens)  →  src/styles/tokens.css, tokens.js  →  컴포넌트
     원본 Variables          게시/Export              Export된 결과물                인스턴스
```

- `brand.600` (원시 색) ← `semantic.primary` (시맨틱 색, 별칭) ← Primary 버튼
- 다크 테마는 `tokens/themes/dark.json`에 "다크일 때 덮어쓸 색"만 따로 있다.

## 실수에서 배운 것

처음에 `src/styles/tokens.js`의 색을 고쳤다. 이건 **Export한 PNG를 직접 고친 것**과 같다. 다시 빌드하면 원본(`tokens.json`) 기준으로 덮어써져서 사라진다.
→ 값은 항상 **원본 `tokens/tokens.json`** 에서 바꾼다. 자동으로 만들어지는 파일은 손대지 않는다.

## 자주 열 파일 (레이어 위치)

| 파일 | Figma로 치면 |
|---|---|
| `tokens/tokens.json` | Variables 패널 (색·간격·반경·글꼴의 단일 원본) |
| `src/components/ds-*.ts` | 메인 컴포넌트 |
| `index.html` | 컴포넌트를 늘어놓은 데모 프레임 |
| `CLAUDE.md` | Claude에게 건네는 Figma↔코드 이름 규칙 |
