# Documentation

> Reference: [GitHub Spec Kit](https://github.com/github/spec-kit), [Anthropic Best Practices](https://code.claude.com/docs/en/best-practices), [AGENTS.md](https://agents.md/)

## 채택 양식

| 양식 | 용도 | 템플릿 |
|------|------|--------|
| **GitHub Spec Kit** | 기능 개발 라이프사이클 | `/templates/spec-kit/` |
| **AGENTS.md** | AI 에이전트 범용 가이드 | `/templates/AGENTS.md` |
| **CLAUDE.md** | Claude Code 전용 지시 | `/templates/CLAUDE.md` |
| **DESIGN.md** | UI 디자인 시스템 | `/templates/DESIGN.md` |
| **ADR** | 아키텍처 결정 기록 | `/templates/adr-template.md` |
| **RFC** | 대규모 기술 변경 제안 | `/templates/rfc-template.md` |

## Spec-Driven Development 워크플로우

> Reference: [GitHub Blog - SDD](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)

```
Constitution → Spec → Plan → Tasks → Code
```

### 1. Constitution (프로젝트 시작 시 1회)

프로젝트의 비협상적 핵심 원칙 정의.
- 기술 스택, 테스트 전략, 코딩 규칙
- 모든 후속 Spec/Plan이 이 원칙을 따름

### 2. Spec (기능별)

Given/When/Then 시나리오 기반 기능 명세.
- 유저 스토리 + 수용 시나리오
- 엣지 케이스 명시
- 기능/비기능 요구사항

### 3. Plan (기능별)

Phase별 구현 계획.
- 기술 컨텍스트 (언어, 의존성, 저장소)
- Constitution 준수 확인
- 위험 및 완화 방안

### 4. Tasks (기능별)

체크박스 태스크 목록.
- `[P]` 표시로 병렬 실행 가능 태스크 표시
- AI 에이전트가 하나씩 처리

## CLAUDE.md 작성 가이드

> Reference: [HumanLayer](https://www.humanlayer.dev/blog/writing-a-good-claude-md), [ETH Zurich 연구 (2026.02)](https://www.infoq.com/news/2026/03/agents-context-file-value-review/)

### 핵심 원칙

1. **60줄 이하 유지** — 80줄 초과 시 Claude가 일부 지시를 무시 시작
2. **AI가 추론 불가능한 정보만** — 커스텀 빌드 명령, 비표준 설정, 프로젝트 고유 제약
3. **자동 생성(/init) 사용 금지** — 직접 작성해야 효과적 (Addy Osmani)
4. **실수에서 진화** — AI가 실수할 때마다 규칙 추가 (Boris Cherny 황금률)
5. **코드 스타일은 린터에** — CLAUDE.md에 스타일 가이드 넣지 말 것

### 3-Tier 구조

| 레벨 | 경로 | Git |
|------|------|-----|
| Global | `~/.claude/CLAUDE.md` | X |
| Project | `.claude/CLAUDE.md` | O |
| Local | `.claude/local.md` | X |

## AGENTS.md 작성 가이드

> Reference: [AGENTS.md 공식](https://agents.md/)

- **CLAUDE.md가 없으면** Claude Code가 AGENTS.md를 폴백으로 읽음
- **범용성:** 모든 AI 코딩 도구 (Copilot, Codex, Cursor, Gemini CLI) 호환
- **내용:** 빌드 명령, 테스트 실행법, 코딩 컨벤션, PR 규칙, 아키텍처 개요

## Documentation-as-Code

- 문서는 코드와 같은 리포에 마크다운으로 관리
- 문서 변경도 PR로 리뷰
- 코드 변경 시 관련 문서도 함께 업데이트 (같은 PR)
