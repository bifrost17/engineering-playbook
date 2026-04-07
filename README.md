# Engineering Playbook

신규 프로젝트 개발 시 기준이 되는 엔지니어링 표준 플레이북.

문서화, 테스트, 코딩 규칙, AI 기반 개발 워크플로우 등 소프트웨어 엔지니어링 전반의 가이드라인을 다룹니다.

## Quick Start

1. [체크리스트](checklist.md)를 먼저 확인하세요
2. 필요한 섹션을 참조하세요
3. [templates/](templates/)에서 프로젝트 템플릿을 복사해서 사용하세요

## Structure

```
docs/
  source-control/              Git 브랜치 전략, 커밋 컨벤션, PR 가이드
  code-reviews/                리뷰 프로세스, 체크리스트, 측정 지표
  coding-conventions/          언어별 스타일 가이드, 린터 설정
  testing/                     단위/통합/E2E/성능 테스트 전략
  ci-cd/                       파이프라인 설계, GitOps, DevSecOps
  design/                      아키텍처 패턴, ADR, 설계 리뷰
  documentation/               문서화 표준 (spec.md, CLAUDE.md 등)
  security/                    애플리케이션/인프라 보안
  observability/               모니터링, 로깅, 트레이싱
  developer-experience/        로컬 환경 설정, 온보딩
  ai-assisted-development/     AI 코딩 가이드, 에이전트 워크플로우
  incident-response/           장애 대응, 포스트모템

templates/                     재사용 템플릿 (CLAUDE.md, AGENTS.md, spec, ADR, RFC, PR)
linter-configs/                공유 린터/포맷터 설정
research/                      리서치, 분석 보고서, 벤치마크
```

## References

- [Microsoft Code-With Engineering Playbook](https://github.com/microsoft/code-with-engineering-playbook)
- [thoughtbot Guides](https://github.com/thoughtbot/guides)
- [GitHub Spec Kit](https://github.com/github/spec-kit)
- [AGENTS.md Standard](https://agents.md/)
