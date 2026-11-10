---
name: figma-to-atomic
description: Figma 디자인을 Atomic CSS 클래스로 구현할 때 사용. Figma URL, 시안 반영, 디자인 수정 요청에 적용. Use when implementing a Figma design with Atomic CSS classes.
argument-hint: "[Figma URL 또는 반영할 디자인]"
---

# Figma → Atomic CSS

> 디자인 데이터는 Figma MCP(`figma-design-to-code`)로 읽고, 이 스킬은 **값을 Atomic 클래스로 옮기는 규칙**만 다룬다.
> 클래스 문법은 `atomic` 스킬, 이름은 `markup-convention:markup`. 반응형·에셋·상태·구조 같은 작업 방침은 사용하는 쪽 규칙을 따른다.

## 순서

1. `figma-design-to-code`로 `get_design_context` + 스크린샷 확보. 스크린샷이 시각 기준이다
2. 응답이 크거나 희소하면 자식 노드를 나눠 다시 요청한다 (필요한 섹션만). 컴포넌트 인스턴스는 그 ID로 따로 요청
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
| 가로 나열 | `dg gtcauto-auto gap{값}` (칸 수만큼 `auto`) |
| 버튼·칩 안 아이콘 + 텍스트 | `df aic gap{값}` |
| 균등 채움 | `dg gtcr{n}-1fr` |
| 양쪽 정렬 | `dg gtc1fr-auto` |
| Fill container | `w100p` / `fg1` |
| Hug | 클래스 없음 |
| 부모 높이 채움 | 부모부터 `h100p` 체인, `dg gtrauto-1fr`, Flex 자식 `fg1 minh0` (`atomic` 스킬) |
| 안쪽 스크롤 영역 (표 래퍼) | `minh0 oya` |
| 절대 좌표 자유 배치 | 행별로 묶고 폭 비율로 `gtc150fr-302fr-112fr` |
| 오버레이·배지 | 부모 `pr` + 자식 `pa t.. l..` |

- 레이아웃은 Grid가 기본이다. `dg` + `gtc`로 칸을 먼저 정한다. Flex(`df`)는 버튼·칩 안 아이콘과 텍스트처럼 한 줄 안의 작은 정렬에만 쓴다. Figma MCP가 `flex`로 줘도 이 기준으로 다시 고른다
- 형제 사이 간격은 부모 gap으로, gap이 없으면 개별 margin으로 반드시 반영한다
- 구분선(line, 얇은 rectangle)은 border나 `hr`로 반드시 옮긴다
- 전역 스타일(`input[type=...]` 등 요소 셀렉터)이 클래스보다 우선하면 셀렉터를 결합(`input.클래스`)해 덮어쓴다. `!important`는 쓰지 않는다

## 확인

- [ ] 스크린샷과 나란히 비교해 위치·크기·색이 일치
- [ ] 디자인에 보이는 요소(탭, 배지, 구분선, 아이콘) 누락 없음
- [ ] `validate_classes` 통과 후 CSS 생성 확인

## 사용자 요청

$ARGUMENTS
