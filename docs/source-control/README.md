# Source Control

> Reference: [Google Engineering Practices](https://google.github.io/eng-practices/), [Conventional Commits](https://conventionalcommits.org), [DORA State of DevOps](https://dora.dev)

## 브랜칭 전략

> **[DECISION NEEDED]** GitHub Flow vs Trunk-Based Development
> - 소규모 팀: GitHub Flow로 시작 권장
> - CI/CD 성숙 후: Trunk-Based + Feature Flags 전환 고려
> - DORA 연구: Trunk-Based가 고성과 팀의 특징으로 일관 보고

### GitHub Flow (기본 권장)

```
main ──────────────────────────────────────────
  └── feature/xxx ──── PR ──── merge ──── delete
```

- `main`은 항상 배포 가능 상태
- 기능별 브랜치 생성 → PR → 리뷰 → 머지 → 브랜치 삭제
- 브랜치 네이밍: `feature/`, `fix/`, `docs/`, `refactor/`

## 커밋 컨벤션

**Conventional Commits** 채택 (업계 사실상 표준).

```
type(scope): description

[optional body]

[optional footer]
```

### 타입

| Type | 설명 |
|------|------|
| `feat` | 새 기능 |
| `fix` | 버그 수정 |
| `docs` | 문서 변경 |
| `refactor` | 리팩토링 (기능 변경 없음) |
| `test` | 테스트 추가/수정 |
| `chore` | 빌드, 도구, 설정 변경 |
| `ci` | CI/CD 설정 변경 |
| `perf` | 성능 개선 |

### 규칙

- 제목: 50자 이내, 소문자, 마침표 없음, 명령형
- 본문: 72자 줄바꿈, "왜(Why)" 설명
- Breaking change: `feat!:` 또는 footer에 `BREAKING CHANGE:`
- 도구: `commitlint` + `husky` (pre-commit hook)

## Pull Request

### 규칙

- **400줄 이하** (Google 연구: 초과 시 리뷰 품질 급락)
- 1 PR = 1 논리적 변경 (Single Responsibility)
- PR 설명에 "왜(Why)" 포함 필수
- Draft PR로 초기 피드백 받기
- CI 통과 전 머지 불가

## 브랜치 보호 규칙

`main` 브랜치에 필수 적용:

- [ ] 직접 push 금지
- [ ] PR 필수, 최소 1명 승인
- [ ] CI 파이프라인 통과 필수
- [ ] Force push 금지
