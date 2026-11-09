---
name: atomic
description: Atomic CSS 클래스로 UI를 작성·수정·검증할 때 사용. 마크업, 레이아웃, 반응형, 클래스 변환 요청에 적용. Use when writing, converting, or validating Atomic CSS utility classes (CSS property + value initials, e.g. display:flex → df).
argument-hint: "[만들 컴포넌트나 바꿀 CSS]"
---

# Atomic CSS

> CSS 속성 + 값의 앞글자를 조합하면 클래스명이 된다. `display: flex` → `df`, `justify-content: center` → `jcc`
> 문법 전체는 MCP `get_guide`가 기준이다. MCP를 쓸 수 없을 때만 [reference.md](reference.md) (atomic 빌드에서 자동 생성)

## 원칙

1. **클래스를 추측하지 않는다** — 아는 클래스도 MCP로 검증한다
2. **스타일은 클래스로** — 인라인 `style` 금지. 클래스로 표현할 수 없는 것(전역 스타일 덮어쓰기, keyframes, 서드파티 DOM)만 프로젝트 스타일시트에 둔다
3. **디자인 값 그대로** — 색상·크기·간격을 반올림하거나 임의로 바꾸지 않는다
4. **클래스는 리터럴 문자열** — CLI가 소스에서 클래스를 추출하므로 조합 문자열(`` `w${n}px` ``)은 생성되지 않는다. 조건부는 완성된 문자열끼리 분기한다

## 작업 순서 (atomic-css MCP)

1. **문법 확인** — 세션에서 처음이면 `get_guide`
2. **변환** — 필요한 CSS 선언을 `css_to_classes`로 변환. 모르는 접두사는 `search_by_css`, 의심되는 클래스는 `lookup_class`
3. **작성** — 검증된 클래스만 사용
4. **검증** — 작성한 클래스를 모아 `validate_classes`. 무효 클래스는 제안대로 수정
5. **생성** — 프로젝트의 CSS 생성 명령(`atomic-css build ...`)을 실행해 실제 CSS에 들어갔는지 확인. 설정 파일 `.atomic.json`이 없어도 새로 만들지 않고 알린다

## 자주 틀리는 것

- `fs0` = flex-shrink: 0 / `fs16px` = font-size: 16px (단위 유무로 속성이 바뀜)
- `p` 단위는 % (`w50p`), rem은 소수점을 `-`로 (`fs1-8rem`)
- 20px 미만은 px, 20px 이상은 rem이 기본 (1rem = 10px). 프로젝트 기준이 다르면 프로젝트를 따른다
- 미디어 프리픽스는 max-width. 데스크톱 값을 먼저 쓰고 작은 화면 값을 덮어쓴다 (`gtcr3-1fr md-gtcr2-1fr sm-gtc1fr`)
- 디자인 폭 비율 Grid는 클래스로 된다: `gtc150fr-302fr-112fr`
- Grid 자식이 넘치면 부모에 `gtc100p` 또는 자식에 `minw0`
- 고정 폭은 `maxw100p`를 같이 준다: `w64rem maxw100p`

## 검증 체크리스트

- [ ] `validate_classes` 통과
- [ ] 인라인 `style` 0개
- [ ] 조합 문자열로 만든 클래스 없음
- [ ] 색상이 디자인 HEX와 정확히 일치
- [ ] CSS 생성 후 실제 규칙이 들어갔는지 확인

## 사용자 요청

$ARGUMENTS
