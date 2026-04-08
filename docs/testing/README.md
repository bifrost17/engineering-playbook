# Testing

> Reference: [Martin Fowler - TestPyramid](https://martinfowler.com/bliki/TestPyramid.html), [Kent C. Dodds - Testing Trophy](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications), [Software Engineering at Google Ch.12](https://abseil.io/resources/swe-book), [ISO/IEC 29119-4](https://www.iso.org/standard/60245.html)

## 핵심 원칙

- **모든 PR에 관련 테스트 포함** — 코드 변경 = 테스트 변경
- **CI에서 자동 실행**, 실패 시 머지 불가
- **Flaky test는 즉시 격리 및 수정**
- **모든 테스트는 구체적 assertion 필수** — `toBeDefined()`, `toBeTruthy()` 같은 약한 assertion만으로는 불충분

## 커버리지

- **새 코드:** 80-90%
- **전체 프로젝트:** 70-80% 유지
- 커버리지 수치보다 **중요 경로(critical path) 커버리지**가 더 중요
- CI에서 커버리지 게이트 설정

### Mutation Testing 목표 (선택)

라인 커버리지보다 정확한 테스트 품질 지표. 테스트가 코드 변경(mutant)을 감지하는지 측정.

| 코드 중요도 | 목표 Mutation Score |
|------------|-------------------|
| 보안 관련 (인증, 파싱) | 90% |
| 비즈니스 로직 | 80% |
| 유틸리티/헬퍼 | 70% |

도구: [Stryker](https://stryker-mutator.io/) (JS/TS), [mutmut](https://github.com/boxed/mutmut) (Python)

## 테스트 유형

| 유형 | 범위 | 속도 | 신뢰도 |
|------|------|------|--------|
| **Unit** | 단일 함수/클래스 | 빠름 | 낮음 |
| **Integration** | 컴포넌트 간 상호작용 | 중간 | 높음 |
| **E2E** | 전체 사용자 시나리오 | 느림 | 가장 높음 |

## 테스트 비율 모델

프로젝트 특성에 따라 선택한다. 결정 시 ADR로 기록할 것.

**기본값: Test Pyramid** (백엔드) / **Test Trophy** (프론트엔드)

| 모델 | 적합한 상황 |
|------|------------|
| **Test Pyramid** (Unit 중심) | 라이브러리, 백엔드 API, 알고리즘 |
| **Test Trophy** (Integration 중심) | 프론트엔드 앱, React/Vue |
| **Test Diamond** (Integration 최다) | 마이크로서비스, API 중심 |

2024-2026 트렌드: Integration 테스트 비중 확대가 공통.

## 테스트 작성 시점

팀 역량과 프로젝트 성격에 따라 선택한다.

**기본값: Write-Tests-After + "PR에 테스트 포함 필수" 규칙**

- **TDD:** 설계 품질 향상, 진입장벽 높음 (~15-20% 채택)
- **Write-Tests-After:** 현실적, 진입장벽 낮음 (~60-70% 채택)
- **실용적 접근:** "PR에 테스트 포함 필수" 규칙이 TDD 강제보다 실효성 높음

---

## 테스트 작성 기준

### F.I.R.S.T 원칙

> Reference: Robert C. Martin, *Clean Code*

| 원칙 | 의미 |
|------|------|
| **Fast** | 빠르게 실행. 느린 테스트는 안 돌림 |
| **Independent** | 테스트 간 의존성 없음, 순서 무관 |
| **Repeatable** | 어떤 환경에서든 동일 결과 |
| **Self-validating** | Pass/Fail 자동 판단 (사람이 로그 확인 불필요) |
| **Timely** | 프로덕션 코드와 함께 작성 |

### 구조: Arrange-Act-Assert (AAA)

```
// Arrange: 테스트 데이터 준비
// Act: 테스트 대상 실행
// Assert: 결과 검증
```

### 네이밍

테스트 이름만으로 무엇을 검증하는지 알 수 있어야 한다.

```
// 패턴: 함수명: 조건 → 결과
formatFileSize: exactly 1024 bytes → "1.0 KB"
createUser: duplicate email → throws ConflictError

// 또는
should_return_404_when_user_not_found
```

### describe 블록 구조

```
describe('functionName', () => {
  describe('happy path', () => { ... })
  describe('boundary values', () => { ... })
  describe('special values', () => { ... })
  describe('error cases', () => { ... })
})
```

---

## 경계값 분석 (Boundary Value Analysis)

> Reference: Ostrand & Balcer 1988, ISO/IEC 29119-4

숫자나 크기 기반 경계가 있는 함수는 경계값을 반드시 테스트한다.

### 7-Point Method

| 포인트 | 설명 | 예시 (경계 = 18) |
|--------|------|-----------------|
| min−1 | 하한 바로 아래 → 거부/클램핑 | `17` |
| min | 정확한 하한 → 수용 | `18` |
| min+1 | 하한 바로 위 | `19` |
| nominal | 전형적 중간값 | `35` |
| max−1 | 상한 바로 아래 | `99` |
| max | 정확한 상한 → 수용 | `100` |
| max+1 | 상한 바로 위 → 거부/클램핑 | `101` |

**최소 필수:** min−1, min, max, max+1 (4개). 이것만으로도 off-by-one 오류를 잡을 수 있다.

---

## 동등 분할 (Equivalence Partitioning)

> Reference: Myers 1979, *The Art of Software Testing*

입력을 함수가 동일하게 처리하는 그룹으로 나누고, 각 그룹에서 대표값을 테스트한다.

### 타입별 필수 테스트값

#### 숫자

| 클래스 | 대표값 |
|--------|--------|
| Zero | `0` |
| 양수 | `1`, `42` |
| 음수 | `-1` |
| 특수값 | `NaN`, `Infinity`, `-Infinity` |
| 소수점 | `0.1`, `1.5` |
| 경계 | 도메인 min−1, min, max, max+1 |

#### 문자열

| 클래스 | 대표값 |
|--------|--------|
| 빈 문자열 | `""` |
| 공백만 | `" "`, `"\t"`, `"\n"` |
| 단일 문자 | `"a"` |
| 일반 ASCII | `"hello"` |
| 유니코드 | `"日本語"`, `"café"`, `"😀"` |
| 매우 긴 문자열 | `"a".repeat(10000)` |

#### Optional (T | null | undefined)

4가지 모두 테스트: 생략(인자 없음), `null`, `undefined`, 유효값. `null`과 `undefined`를 동등하게 취급하지 않는다.

#### Boolean

`true`와 `false` 모두. 타입이 `any`면 `0`, `1`, `null`, `undefined`도 추가.

#### 배열/컬렉션

| 클래스 | 대표값 |
|--------|--------|
| 빈 배열 | `[]` |
| 단일 요소 | `[item]` |
| 일반 | `[a, b, c]` |
| 중복 포함 | `[a, a, b]` |

---

## 함수 유형별 요구사항

### 포맷터 함수 (값 → 문자열)

- 모든 출력 분기 테스트 (예: B, KB, MB, GB)
- 단위 전환 경계의 양쪽 테스트 (n−1, n, n+1)
- `0` 별도 테스트 (보통 고유한 포맷)
- **정확한 문자열 출력 assert** (`toBe("1.0 KB")` — `toBeDefined()` 아님)

### Boolean/Predicate 함수

- `true` 리턴 입력과 `false` 리턴 입력 모두 테스트
- 결과가 뒤집히는 정확한 경계 입력 테스트
- 복합 조건의 각 측 독립 테스트:

```
// canAccess = isLoggedIn && hasPermission 인 경우
(true, true)   → true   // happy path
(true, false)  → false  // && → || 뮤턴트 킬
(false, true)  → false  // && → || 뮤턴트 킬
(false, false) → false  // baseline
```

### 보안 관련 파서/검증 함수

- 정상 입력 → 통과
- 알려진 공격 패턴 → 거부: `;`, `|`, `&`, `$()`, `` ` `` (셸 인젝션), `../../` (경로 탐색)
- OWASP 가이드 기반 메타문자 테스트

---

## Parameterized Test (test.each)

3개 이상의 케이스가 **동일한 assertion 패턴**을 공유하면 `test.each` 사용.

```typescript
// GOOD: 같은 패턴, 다른 데이터
test.each([
  { input: 0,    expected: '0 B'    },
  { input: 1023, expected: '1023 B' },
  { input: 1024, expected: '1.0 KB' },
])('formatFileSize($input) === $expected', ({ input, expected }) => {
  expect(formatFileSize(input)).toBe(expected)
})
```

### 하지 말 것

- 3개 미만이면 개별 `it()` 사용
- 서로 다른 assertion 타입 (리턴값 검증 / throw 검증)을 하나에 합치지 않음
- 50개 이상 행은 의미 단위로 여러 `test.each`로 분리

---

## 안티패턴

| 안티패턴 | 이유 |
|---------|------|
| `expect(result).toBeDefined()` 만 사용 | 모든 값/문자열 변이에서 살아남음 |
| `expect(result).toBeTruthy()` (숫자) | 산술 변이에서 살아남음 |
| `expect()` 없이 함수만 호출 | 검증 가치 제로 |
| 계산된 기대값 사용 `[n, double(n)]` | 원본 함수가 깨지면 기대값도 틀림 |
| throw 케이스와 리턴값 케이스 혼합 | 의도 불명확, 네이밍 불가 |
| Mock 호출 횟수만 검증, 인자 미검증 | 잘못된 Mock이 호출돼도 통과 |
| 환경 변수 변경 후 복원 안 함 | 다른 테스트에 영향 |
| 구현 세부사항 테스트 | 리팩토링 시 깨짐 |
| Sleep/시간 의존 | Flaky test |
| 외부 서비스 직접 호출 | 환경 의존, 느림 |

---

## 새 테스트 파일 체크리스트

- [ ] 모든 `it()`에 구체적 값의 `expect()` 포함
- [ ] 빈값/0 입력 커버 (파라미터 타입별)
- [ ] 특수값 최소 1개 (NaN, 유니코드, null byte, 빈 문자열)
- [ ] 숫자 경계가 있는 함수는 경계값 테스트
- [ ] Boolean 함수는 true/false 양쪽 테스트
- [ ] 복합 조건 (`&&`/`||`)은 각 측 독립 테스트
- [ ] 3개 이상 동일 패턴이면 `test.each` 사용
- [ ] 테스트 이름이 `함수: 조건 → 결과` 형식
- [ ] 환경 변수 변경 시 afterAll에서 복원
- [ ] 포맷터는 정확한 문자열 출력 assert
