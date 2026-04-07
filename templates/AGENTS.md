# AGENTS.md

> **Reference:** [AGENTS.md Standard](https://agents.md/) (Linux Foundation Agentic AI Foundation)
> 
> AI 코딩 에이전트가 이 프로젝트에서 작업할 때 참조하는 범용 가이드.
> Claude Code는 CLAUDE.md가 없을 경우 이 파일을 폴백으로 읽습니다.

## Dev Environment

- **Language:**
- **Package Manager:**
- **Build:** `command here`
- **Dev Server:** `command here`

## Testing

- **Test Runner:**
- **Run Tests:** `command here`
- **Lint:** `command here`
- **Type Check:** `command here`

## Code Conventions

- **Naming:** (예: camelCase for variables, PascalCase for components)
- **File Structure:** (예: feature-based, co-locate tests)
- **Error Handling:** (예: use Result type, no silent catches)
- **Imports:** (예: absolute imports, group by external/internal)

## PR Instructions

- **Title Format:** (예: `type(scope): description`)
- **Before Commit:**
  - [ ] `lint command`
  - [ ] `test command`
  - [ ] `type-check command`

## Architecture

[주요 컴포넌트 간 관계 설명]

## Important Rules

- (예: DO NOT modify files in `/generated/`)
- (예: Always use transactions for DB writes)
- (예: API responses must follow the shared schema)
