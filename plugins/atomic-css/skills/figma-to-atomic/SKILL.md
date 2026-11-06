---
name: figma-to-atomic
description: Figma 디자인을 Atomic CSS 클래스로 구현할 때 사용. Figma URL, 시안 반영, 디자인 수정 요청에 적용. Use when implementing a Figma design with Atomic CSS classes.
argument-hint: "[Figma URL 또는 반영할 디자인]"
---

# Figma → Atomic CSS

> 디자인 데이터는 공식 Figma 스킬(`figma-design-to-code`)로 읽고, 이 스킬은 **값을 Atomic 클래스로 옮기는 규칙**만 다룬다.
> 클래스 문법은 `atomic` 스킬, 구조와 접근성은 `markup` 스킬을 따른다.

## 순서

1. `figma-design-to-code`로 `get_design_context` + 스크린샷 확보. 스크린샷이 시각 기준이다
2. 응답이 크거나 희소하면 자식 노드를 나눠 다시 요청한다 (필요한 섹션만)
3. 반환된 코드에서 CSS 값을 추출해 `css_to_classes`로 변환한다
4. 프로젝트 구조(컴포넌트, 기존 클래스, 에셋)에 맞게 다시 쓴다. 반환 코드를 그대로 붙이지 않는다
5. `validate_classes` → CSS 생성 → 스크린샷과 대조

## 값 옮기기

- **색상** — HEX 그대로, 6자리 대문자. 비슷한 색(`F5F5F5`/`FAFAFA`)을 혼동하지 않는다
- **크기·간격·폰트** — 디자인 값 그대로. 반올림·배수 맞춤 금지
- **단위** — 20px 미만 px, 20px 이상 rem (1rem = 10px). 예: 18px → `fs18px`, 24px → `gap2-4rem`
- **CSS 변수 fallback** — `var(--name, 값)`은 fallback 값을 쓴다

### Tailwind 출력 → Atomic

| Figma MCP 출력 | Atomic |
|---|---|
| `bg-[var(--x,#0f172a)]` | `bg0F172A` |
| `bg-[var(--x,rgba(0,0,0,0.6))]` | `bg0-0-0-60` |
| `border border-[#2e3647] border-solid` | `b1pxsolid2E3647` |
| `rounded-[12px]` | `br12px` |
| `p-[20px]` / `px-[16px]` | `p2rem` / `px16px` |
| `gap-[8px]` | `gap8px` |
| `flex flex-col items-center justify-between` | `df fdc aic jcsb` |
| `text-[15px] leading-[25px]` | `fs15px lh25px` |
| `font-['Pretendard:Bold']` / `Medium` | `fw700` / `fw500` |
| `tracking-[-0.18px]` | `neg-ls0-18px` |
| `text-[color:var(--x,#9ca3af)]` | `c9CA3AF` |
| `whitespace-nowrap` / `overflow-clip` / `shrink-0` | `wsn` / `oh` / `fs0` |
| `flex-[1_0_0]` / `w-full` / `size-[52px]` | `fg1` / `w100p` / `w52px h52px` |

표에 없는 것은 `css_to_classes`로 변환한다.

## 레이아웃

| Figma | Atomic |
|---|---|
| 세로 Auto Layout + gap | `dg gap{값}` |
| 가로 나열 | `df aic gap{값}` 또는 `dg gtcauto-auto` |
| 균등 채움 | `dg gtcr{n}-1fr` |
| 양쪽 정렬 | `dg gtc1fr-auto` 또는 `df jcsb` |
| Fill container | `w100p` / `fg1` |
| Hug | 클래스 없음 |
| 절대 좌표 자유 배치 | 행별로 묶고 폭 비율로 `gtc150fr-302fr-112fr` |
| 오버레이·배지 | 부모 `pr` + 자식 `pa t.. l..` |

- 형제 사이 간격은 부모 gap으로, gap이 없으면 개별 margin으로 반드시 반영한다
- 구분선(line, 얇은 rectangle)은 border나 `hr`로 반드시 옮긴다
- 반응형 시안이 없으면 축소 규칙(`md-`, `sm-`)으로 대응하되, 별도 모바일 디자인이 예정된 영역은 임의로 만들지 않는다

## 에셋

- 다운로드·재사용·크기 규칙은 `figma-design-to-code`를 따른다
- 색만 다른 상태(밝은 배경용 등)가 필요하면 같은 에셋의 색상 변형을 만든다

## 상태

- 컴포넌트 그룹에서 하나만 모양이 다른 항목은 그 컴포넌트의 hover/active 상태다
- 디자인에 없는 hover 색, 기본 선택 상태를 만들지 않는다
- 입력 가이드(기본·포커스·오류·비활성)가 있으면 그 값을 그대로 쓴다

## 기존 코드에 적용할 때

- 이벤트, 상태, 데이터 바인딩, props, ref는 유지하고 클래스만 바꾼다
- 디자인에 없더라도 기능이 있는 요소는 지우지 않는다
- 전역 스타일(`input[type=...]` 등 요소 셀렉터)이 클래스보다 우선하면 프로젝트 스타일시트에서 `input.클래스`처럼 셀렉터를 결합해 덮어쓴다. `!important`는 쓰지 않는다

## 확인

- [ ] 스크린샷과 나란히 비교해 위치·크기·색이 일치
- [ ] 디자인에 보이는 요소(탭, 배지, 구분선, 아이콘) 누락 없음
- [ ] `validate_classes` 통과 후 CSS 생성 확인
- [ ] 모호한 부분만 질문하고 나머지는 진행

## 사용자 요청

$ARGUMENTS
