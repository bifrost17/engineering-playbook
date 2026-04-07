# Engineering Playbook

신규 프로젝트 개발 시 기준이 되는 엔지니어링 표준 플레이북.

문서화, 테스트, 코딩 규칙, AI 기반 개발 워크플로우 등 소프트웨어 엔지니어링 전반의 가이드라인을 다룹니다.

## Adopted Standards

이 플레이북은 다음 4가지 경량 문서화 양식을 채택합니다:

| 양식 | 용도 | 레퍼런스 |
|------|------|---------|
| **[GitHub Spec Kit](https://github.com/github/spec-kit)** | Spec-Driven Development (Constitution → Spec → Plan → Tasks) | GitHub 공식 |
| **[AGENTS.md](https://agents.md/)** | AI 에이전트 범용 프로젝트 가이드 | Linux Foundation AAIF |
| **[DESIGN.md](https://github.com/VoltAgent/awesome-design-md)** | AI 에이전트용 UI 디자인 시스템 | Google Stitch 포맷 |
| **[ADR](https://adr.github.io/madr/)** | 아키텍처 의사결정 기록 | MADR + Michael Nygard |

## Quick Start

1. [체크리스트](checklist.md)를 먼저 확인하세요
2. 필요한 섹션을 `docs/`에서 참조하세요
3. `templates/`에서 프로젝트 템플릿을 복사해서 사용하세요

## Structure

```
docs/
  source-control/              Git 브랜치 전략, 커밋 컨벤션, PR 가이드
  code-reviews/                리뷰 프로세스, 체크리스트, 측정 지표
  coding-conventions/          언어별 스타일 가이드, 린터 설정
  testing/                     단위/통합/E2E/성능 테스트 전략
  ci-cd/                       파이프라인 설계, GitOps, DevSecOps
  design/                      아키텍처 패턴, ADR, DESIGN.md 작성법
  documentation/               문서화 표준, Spec Kit 워크플로우 가이드
  security/                    애플리케이션/인프라 보안
  observability/               모니터링, 로깅, 트레이싱
  developer-experience/        로컬 환경 설정, 온보딩
  ai-assisted-development/     AI 코딩 가이드, AGENTS.md/CLAUDE.md 작성법
  incident-response/           장애 대응, 포스트모템

templates/
  spec-kit/                    GitHub Spec Kit 4파일 세트
    constitution.md              프로젝트 핵심 원칙
    spec.md                      기능 명세 (Given/When/Then)
    plan.md                      구현 계획 (Phase별)
    tasks.md                     태스크 목록 (체크박스)
  AGENTS.md                    AI 에이전트 범용 가이드
  CLAUDE.md                    Claude Code 전용 지시 파일
  DESIGN.md                    UI 디자인 시스템
  adr-template.md              아키텍처 결정 기록
  rfc-template.md              기술 제안서
  pr-template.md               Pull Request

linter-configs/                공유 린터/포맷터 설정
research/                      리서치, 분석 보고서, 벤치마크
```

## References

### Adopted Standards
- [GitHub Spec Kit](https://github.com/github/spec-kit) - Spec-Driven Development
- [AGENTS.md Standard](https://agents.md/) - Linux Foundation Agentic AI Foundation
- [DESIGN.md (awesome-design-md)](https://github.com/VoltAgent/awesome-design-md) - Google Stitch 포맷
- [MADR (Markdown Any Decision Records)](https://adr.github.io/madr/) - 아키텍처 결정 기록

### Playbook References
- [Microsoft Code-With Engineering Playbook](https://github.com/microsoft/code-with-engineering-playbook)
- [thoughtbot Guides](https://github.com/thoughtbot/guides)
- [Anthropic Claude Code Best Practices](https://code.claude.com/docs/en/best-practices)
