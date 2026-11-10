---
name: markup
description: 마크업의 class·id·파일·경로 이름을 정하거나 Convention Lint로 네이밍을 검사할 때 사용. Use when naming classes, ids, files, or asset paths in markup and validating them with the Convention Lint MCP.
argument-hint: "[검사할 마크업이나 정할 이름]"
---

# 마크업 네이밍 (Convention Lint)

> 이 스킬은 Convention Lint MCP가 검사하는 네이밍 규칙과 도구 사용법만 다룬다.
> 구조·접근성·반응형 같은 작업 방침은 사용하는 쪽 규칙을 따른다. 스타일 클래스는 `atomic-css:atomic`.

## 네이밍 (Convention Lint 기본 설정)

| 대상 | 규칙 | 예시 |
|---|---|---|
| class (의미 클래스) | kebab-case | `site-header`, `table-input` |
| id | camelCase | `btnCreate`, `inputTitle` |
| 문서 파일 (`.html` `.vue` `.jsx` `.tsx` 등) | kebab-case. 컴포넌트 파일은 프로젝트 관례 | `main-visual.html`, `MainVisual.tsx` |
| 에셋 파일 (이미지·폰트·영상·사운드) | snake_case | `gnb_icon_search.svg`, `bg_img_main_1920x1080.jpg` |
| 폴더 | snake_case (한 단어가 기본) | `images`, `icons`, `gnb_icons` |

- 클래스명만 보고 역할을 알 수 있게 쓴다. `tbl-inp`가 아니라 `table-input`, `btn-pub`가 아니라 `btn-publish`
- Atomic CSS 클래스(`df`, `gap8px` 등)는 검사에서 제외된다

## 도구

| 도구 | 용도 |
|---|---|
| `get_guide` | 지원 규칙과 기본 설정 전체 |
| `validate_with_atomic` | class·id 검사. Atomic 클래스를 분리해 진짜 위반만 남김 (Atomic 프로젝트 기본) |
| `validate_text` | class·id·경로 검사. 에셋 파일명·폴더는 이걸로 (`validate_with_atomic`은 경로를 보지 않음) |
| `analyze_violation` | 위반 이유 상세 |
| `convert_name` | 규칙에 맞는 이름으로 변환 |
| `check_atomic` | 클래스가 Atomic CSS인지 확인 |

## 순서

1. 이름을 정할 때 위 표를 따르고, 애매하면 `convert_name`
2. 마크업을 쓴 뒤 `validate_with_atomic`, 새 에셋 경로가 있으면 경로를 넣어 `validate_text`
3. 위반은 `analyze_violation`으로 이유를 보고 고친다. 위반 0이 기준

## 사용자 요청

$ARGUMENTS
