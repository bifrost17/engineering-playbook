# DESIGN.md

> **Reference:** [awesome-design-md](https://github.com/VoltAgent/awesome-design-md) (Google Stitch 포맷)
>
> 프로젝트의 UI 디자인 시스템. AI 에이전트가 일관된 디자인을 생성할 수 있도록 한다.
> AGENTS.md = "어떻게 빌드", DESIGN.md = "어떻게 보여야 하는지"

## 1. Visual Theme & Atmosphere

- **Mood:** (예: clean, minimal, professional)
- **Density:** (예: spacious, compact)
- **Design Philosophy:** (예: content-first, data-dense)

## 2. Color Palette & Roles

| Role | Name | Hex | Usage |
|------|------|-----|-------|
| Primary | | | 주요 CTA, 강조 |
| Secondary | | | 보조 요소 |
| Background | | | 페이지 배경 |
| Surface | | | 카드, 패널 배경 |
| Text Primary | | | 본문 텍스트 |
| Text Secondary | | | 보조 텍스트 |
| Success | | | 성공 상태 |
| Warning | | | 경고 상태 |
| Error | | | 에러 상태 |

## 3. Typography Rules

| Level | Font | Size | Weight | Usage |
|-------|------|------|--------|-------|
| H1 | | | | 페이지 타이틀 |
| H2 | | | | 섹션 타이틀 |
| H3 | | | | 서브섹션 |
| Body | | | | 본문 |
| Caption | | | | 보조 텍스트 |
| Code | | | | 코드 블록 |

## 4. Component Stylings

### Buttons

| Type | Style | States |
|------|-------|--------|
| Primary | | default, hover, active, disabled |
| Secondary | | default, hover, active, disabled |
| Ghost | | default, hover, active, disabled |

### Cards

- Border radius:
- Shadow:
- Padding:

### Inputs

- Border:
- Focus state:
- Error state:

## 5. Layout Principles

- **Spacing Scale:** (예: 4px 기반 - 4, 8, 12, 16, 24, 32, 48, 64)
- **Max Content Width:**
- **Grid:**
- **Margin Philosophy:**

## 6. Depth & Elevation

| Level | Usage | Shadow |
|-------|-------|--------|
| 0 | Flat elements | none |
| 1 | Cards, panels | |
| 2 | Dropdowns, tooltips | |
| 3 | Modals, dialogs | |

## 7. Do's and Don'ts

**Do:**
- 

**Don't:**
- 

## 8. Responsive Behavior

| Breakpoint | Width | Layout |
|------------|-------|--------|
| Mobile | < 768px | |
| Tablet | 768-1024px | |
| Desktop | > 1024px | |

- Touch target minimum:

## 9. Agent Prompt Guide

AI 에이전트가 즉시 사용할 수 있는 빠른 참조:

```
Primary: #___  Secondary: #___  Background: #___
Font: ___  Radius: ___px  Spacing base: ___px
```
