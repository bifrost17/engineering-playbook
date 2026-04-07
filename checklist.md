# Engineering Fundamentals Checklist

신규 프로젝트 시작 시 확인해야 할 엔지니어링 기본 사항.

## Source Control

- [ ] Git 브랜치 전략 결정 (trunk-based / git-flow)
- [ ] 커밋 메시지 컨벤션 합의 (conventional commits 등)
- [ ] PR 템플릿 설정
- [ ] branch protection 규칙 설정

## Code Reviews

- [ ] 리뷰 프로세스 합의 (최소 1명 승인 등)
- [ ] 리뷰 체크리스트 작성

## Coding Conventions

- [ ] 언어별 스타일 가이드 선정
- [ ] 린터/포맷터 설정 (.eslint, .prettier, ruff 등)
- [ ] pre-commit hooks 설정

## Testing

- [ ] 테스트 전략 결정 (단위/통합/E2E 비율)
- [ ] 최소 커버리지 기준 설정
- [ ] CI에서 테스트 자동 실행 설정

## CI/CD

- [ ] CI 파이프라인 구성 (빌드, 테스트, 린트)
- [ ] CD 파이프라인 구성 (스테이징, 프로덕션)
- [ ] 시크릿 관리 방법 결정

## Design & Architecture

- [ ] 아키텍처 결정 기록(ADR) 시작
- [ ] 주요 기술 스택 결정 및 문서화
- [ ] 시스템 아키텍처 다이어그램 작성

## Documentation

- [ ] CLAUDE.md 또는 AGENTS.md 작성
- [ ] spec.md 작성 (프로젝트 명세)
- [ ] plan.md 작성 (구현 계획)

## Security

- [ ] 의존성 취약점 스캔 설정
- [ ] 시크릿 탐지 설정 (git-secrets, gitleaks 등)
- [ ] OWASP Top 10 검토

## Observability

- [ ] 로깅 전략 결정
- [ ] 모니터링/알림 설정
- [ ] 에러 트래킹 도구 설정
