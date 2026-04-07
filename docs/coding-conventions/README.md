# Coding Conventions

> Reference: [Google Style Guides](https://google.github.io/styleguide/)

언어에 무관한 공통 원칙. 언어별 린터 설정은 `/linter-configs/` 참조.

## 핵심 원칙

| 원칙 | 설명 | 적용 |
|------|------|------|
| **KISS** | 단순하게 유지 | 항상 |
| **YAGNI** | 필요할 때 만들기 | 항상 |
| **DRY** | 반복 금지 | 3번 반복 시 추상화 고려 (Rule of Three) |
| **SOLID** | 객체지향 설계 원칙 | 유연한 가이드라인, 절대 규칙 아님 |

> 3줄 중복이 잘못된 추상화보다 낫다.

## 네이밍

- **명확성 > 간결성** — `userAccountList` > `ual`
- **일관성** — 프로젝트 내 동일 개념에 동일 용어
- **Boolean:** `is`, `has`, `can`, `should` 접두어
- **함수:** 동사로 시작 (`get`, `create`, `delete`, `validate`)
- **약어 지양** — 도메인 용어가 아닌 한 피하기

## 에러 처리

- **에러를 삼키지 않는다** — silent catch 금지
- **구체적 에러 타입** 사용 — 범용 Error/Exception 지양
- **에러 메시지에 컨텍스트 포함** — `"Failed to connect to DB at {host}:{port}: {reason}"`
- **복구 가능 vs 불가능 구분** — 복구 가능하면 재시도/폴백, 불가능하면 fail fast
- **사용자 에러와 시스템 에러 분리** — 4xx vs 5xx

## 로깅

**구조화된 로깅 (Structured Logging)** 필수.

```json
{
  "timestamp": "2026-04-07T10:30:00Z",
  "level": "ERROR",
  "service": "user-api",
  "traceId": "abc123",
  "message": "Failed to create user",
  "context": { "userId": "xxx", "reason": "duplicate email" }
}
```

| Level | 용도 | 프로덕션 |
|-------|------|---------|
| DEBUG | 디버깅 | OFF |
| INFO | 정상 동작 | ON |
| WARN | 잠재적 문제 | ON |
| ERROR | 실패, 대응 필요 | ON |

- 민감 정보 (비밀번호, 토큰, PII) 절대 로깅 금지
- 모든 로그에 correlation/trace ID 포함

## 주석

- **WHY만 쓴다** — 코드로 표현할 수 없는 이유, 의도, 제약
- **WHAT은 쓰지 않는다** — 코드 자체가 설명해야 함
- **TODO:** 이슈 번호 포함 (`// TODO(#123): 만료 로직 추가`)
- 주석이 필요하면 코드를 먼저 개선할 수 없는지 고려

## 린터/포맷터

> **[DECISION NEEDED]** 사용 언어에 따라 도구 선택

| 언어 | 포맷터 | 린터 |
|------|--------|------|
| Python | black / ruff format | ruff |
| TypeScript/JS | prettier | eslint |
| Go | gofmt | golangci-lint |
| Rust | rustfmt | clippy |

적용: Pre-commit hook → CI 검사 → Editor 저장 시 포맷팅
