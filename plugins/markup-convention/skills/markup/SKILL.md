---
name: markup
description: HTML/JSX/Vue 마크업을 작성하거나 검수할 때 사용. 구조, 네이밍, 접근성, 반응형 점검에 적용. Use when writing or reviewing markup for structure, naming, and accessibility with Convention Lint.
argument-hint: "[작성하거나 검수할 화면·컴포넌트]"
---

# 마크업 컨벤션

> 구조와 접근성 규칙. 스타일 클래스는 `atomic` 스킬, 디자인 반영은 `figma-to-atomic` 스킬을 따른다.

## 네이밍

| 대상 | 규칙 | 예시 |
|---|---|---|
| class (의미 클래스) | kebab-case | `site-header`, `table-input` |
| id | camelCase | `btnCreate`, `inputTitle` |
| 파일 | kebab-case (컴포넌트 파일은 프로젝트 관례) | `main-visual.html`, `MainVisual.tsx` |

- 클래스명만 보고 역할을 알 수 있게 쓴다. `tbl-inp`가 아니라 `table-input`
- 의미 클래스는 섹션·상태·JS 훅에만 붙인다 (`site-header`, `is-open`, `is-light`)

## 스타일 클래스 전략

| 프로젝트 | 방식 |
|---|---|
| Atomic CSS 사용 | 스타일은 Atomic 클래스. 의미 클래스는 섹션 식별과 스타일시트 덮어쓰기 지점에만 |
| Atomic CSS 미사용 | 섹션 클래스 + SCSS 계층 셀렉터 (`.gnb > ul > li > a`). 하위 요소마다 클래스를 만들지 않는다 |

- 같은 이름의 `scss`/`css`/`min.css`가 있으면 `scss`만 수정한다

## 구조

- **시맨틱 태그** — `header` `nav` `main` `section` `article` `aside` `footer`를 역할에 맞게 쓴다
- **목록은 목록으로** — 메뉴·카드 목록 `ul/li`, 순서 `ol`, 정의 `dl/dt/dd`, 입력 `form/fieldset/legend`, 표 `table`
- **빈 래퍼 금지** — 스타일도 의미도 없는 `div`로 감싸지 않는다. React Fragment, Vue multi-root를 쓴다
- **제목 계층** — `h1`~`h6`를 건너뛰지 않는다
- **버튼과 링크 구분** — 이동은 `a`, 동작은 `button type="button"`
- **a·button 안의 텍스트는 `span`으로 감싼다**

```html
<a href="/notice" class="df aic gap8px">
    <span>공지사항</span>
    <img src="/images/ic-arrow.svg" width="16" height="16" alt="" />
</a>
```

## 접근성

- 텍스트 없는 버튼·링크에는 `aria-label`
- 새 창 링크는 `target="_blank" rel="noopener"` + 새 창임을 알리는 `title`
- 장식 이미지는 `alt=""`, 의미 있는 이미지는 내용을 `alt`에
- 토글은 `aria-expanded`, 선택은 `aria-pressed`/`aria-selected`, 현재 위치는 `aria-current`
- 모달: `role="dialog"` `aria-modal="true"` `aria-labelledby`, 열릴 때 포커스 이동, ESC 닫기, 닫힐 때 원래 위치로 포커스 복귀, 포커스 갇힘
- 키보드로 모든 동작 가능. `focus-visible` 표시를 없애지 않는다
- 폼: `label`과 입력 연결, 오류 메시지는 `aria-describedby`, 필수는 `aria-required`

### 표

```html
<table>
    <caption>공지사항 목록</caption>
    <colgroup><col class="w8rem" /><col /><col class="w12rem" /></colgroup>
    <thead>
        <tr><th scope="col">번호</th><th scope="col">제목</th><th scope="col">등록일</th></tr>
    </thead>
</table>
```

- `caption`(시각적으로 숨겨도 됨), `colgroup/col`, `th scope` 필수
- 모바일은 가로 스크롤 래퍼 또는 카드형 전환

## 반응형

- 모든 레이아웃에 태블릿·모바일 대응을 넣는다 (Atomic이면 `md-`, `sm-` 프리픽스)
- 프로젝트가 정한 최소 폭(예: 320px)에서 가로 스크롤이 생기지 않아야 한다

## 검증 (Convention Lint MCP)

| 도구 | 용도 |
|---|---|
| `validate_text` | 마크업 텍스트의 class·id·경로 네이밍 위반 검사 |
| `validate_with_atomic` | 위반 중 Atomic 클래스를 분리해 진짜 위반만 남김 (Atomic 프로젝트 기본) |
| `analyze_violation` | 위반 이유 상세 |
| `convert_name` | 규칙에 맞는 이름으로 변환 |
| `get_guide` | 지원 규칙 전체 |

체크리스트
- [ ] `validate_with_atomic`(또는 `validate_text`) 위반 0
- [ ] 빈 래퍼 `div` 없음
- [ ] 텍스트 없는 버튼·링크에 `aria-label`
- [ ] 표에 `caption`·`th scope`
- [ ] 모달·드롭다운 키보드 동작
- [ ] 태블릿·모바일 대응

## 사용자 요청

$ARGUMENTS
