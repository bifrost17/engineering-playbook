# Developer Experience

> Reference: [Backstage](https://backstage.io/), [Microsoft Code-With Engineering Playbook](https://microsoft.github.io/code-with-engineering-playbook/)

## 온보딩

### 30-60-90일 프레임워크

| 기간 | 목표 | 체크리스트 |
|------|------|-----------|
| **30일** | 환경 세팅, 첫 PR | 개발 환경 구축, 코드베이스 이해, 작은 버그 수정 PR |
| **60일** | 독립적 기능 개발 | 중간 규모 기능 구현, 코드 리뷰 참여, 아키텍처 이해 |
| **90일** | 기여자 | 설계 논의 참여, ADR 작성, 온콜 참여 |

### 첫째 날 체크리스트

- [ ] 리포지토리 접근 권한
- [ ] 개발 환경 세팅 (아래 참조)
- [ ] CLAUDE.md / AGENTS.md 읽기
- [ ] 이 플레이북 읽기 (최소한 checklist.md)
- [ ] 첫 Good First Issue 할당

## 로컬 개발 환경

### 원칙

- **한 명령으로 시작** — `make dev` 또는 `docker compose up`
- **문서화** — README.md에 설치 가이드 필수
- **일관성** — 팀 전체가 동일 도구 버전 사용

### 도구 버전 관리

| 도구 | 역할 |
|------|------|
| **asdf** 또는 **mise** | 언어/런타임 버전 관리 (`.tool-versions`) |
| **Docker Compose** | 로컬 인프라 (DB, Redis, 메시지 큐) |
| **direnv** | 디렉토리별 환경 변수 자동 로드 |

### 기본 Makefile 패턴

```makefile
.PHONY: dev test lint setup

setup:      ## 최초 환경 세팅
dev:        ## 개발 서버 실행
test:       ## 테스트 실행
lint:       ## 린트 실행
```

## Documentation-as-Code

- 문서는 코드와 같은 리포에서 마크다운으로 관리
- 문서 변경도 PR로 리뷰
- 코드 변경 시 관련 문서도 같은 PR에서 업데이트

## Inner Source

- 팀 간 코드 기여를 장려하는 문화
- `CONTRIBUTING.md`에 기여 가이드 명시
- Good First Issue 라벨로 진입장벽 낮추기
