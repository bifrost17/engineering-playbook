# Testing

> Reference: [Martin Fowler - TestPyramid](https://martinfowler.com/bliki/TestPyramid.html), [Kent C. Dodds - Testing Trophy](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications), [Software Engineering at Google Ch.12-14](https://abseil.io/resources/swe-book), [web.dev - Testing Strategies](https://web.dev/articles/ta-strategies), [ISO/IEC 29119-4](https://www.iso.org/standard/60245.html)

## 핵심 원칙

- **모든 PR에 관련 테스트 포함** — 코드 변경 = 테스트 변경
- **CI에서 자동 실행**, 실패 시 머지 불가
- **Flaky test는 즉시 격리 및 수정**
- **모든 테스트는 구체적 assertion 필수** — `toBeDefined()`, `toBeTruthy()` 같은 약한 assertion만으로는 불충분
- **구현이 아닌 행동을 테스트** — Public API를 통해, 사용자와 동일한 방식으로 호출. 상태(state)를 검증하지 상호작용(interaction)을 검증하지 않는다
- **Real > Fake > Mock** — 실제 구현체를 우선 사용하고, Mock은 최후 수단 (상세: [Test Double 전략](#test-double-전략) 참조)

## 커버리지

- **새 코드:** 80-90%
- **전체 프로젝트:** 70-80% 유지
- 커버리지 수치보다 **중요 경로(critical path) 커버리지**가 더 중요
- CI에서 커버리지 게이트 설정

### 커버리지 수치의 한계

- **70% 이상에서 수익 체감** — 구현 세부사항 테스트를 강제하여 유지보수 부담 증가 (Kent C. Dodds)
- **100% 커버리지는 의심 신호** — 수치 충족을 위한 사소한 코드 테스트를 시사 (Martin Fowler)
- **라이브러리 vs 애플리케이션**: 많은 프로젝트에 영향을 미치는 라이브러리는 높은 커버리지(~100%)가 유익. 애플리케이션은 80%대로 충분
- **진정한 충분성 판단 기준**: (1) 프로덕션 버그가 드물다 (2) 개발자가 코드 변경에 자신감을 느낀다

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

## 테스트 크기 분류 (선택)

> Reference: [Software Engineering at Google Ch.11](https://abseil.io/resources/swe-book/html/ch11.html)

테스트 유형(Unit/Integration/E2E)은 **범위(scope)**, 테스트 크기(Small/Medium/Large)는 **실행 제약(execution constraints)**. 두 개는 독립적인 차원이다. CI 파이프라인 분리 시 유용.

| 크기 | 프로세스 | I/O | 네트워크 | 속도 | 결정성 |
|------|---------|-----|---------|------|--------|
| **Small** | 단일 프로세스 | 금지 | 금지 | 밀리초 | 매우 높음 |
| **Medium** | 다중 프로세스 | localhost만 | localhost만 | 초 단위 | 중간 |
| **Large** | 제한 없음 | 제한 없음 | 원격 허용 | 분 단위 | 낮음 |

**기본값: 별도 분류 없이 유형(Unit/Integration/E2E)으로 관리.** 프로젝트 규모가 커지면 크기 분류로 CI 단계를 분리할 수 있다.

## 테스트 비율 모델

프로젝트 특성에 따라 선택한다. 결정 시 ADR로 기록할 것.

**기본값: Test Pyramid** (백엔드) / **Test Trophy** (프론트엔드)

| 모델 | 핵심 비중 | 적합한 상황 |
|------|----------|------------|
| **Test Pyramid** (Unit 중심) | Unit 최다 | 라이브러리, 백엔드 API, 알고리즘 |
| **Test Trophy** (Integration 중심) | Integration 최다 | 프론트엔드 앱, React/Vue |
| **Test Diamond** (Integration 최다) | Integration 최다 | 마이크로서비스, API 중심 |
| **Test Honeycomb** (Integration 중심) | Integration 최다, Unit 최소 | 마이크로서비스 (서비스 간 상호작용이 핵심) |
| **Testing Crab** (E2E + Visual 중심) | E2E/Visual 최다, Unit 최소 | UI 집약적 앱, 시각적 일관성 중요 |

### 아키텍처별 선택 기준

| 아키텍처 | 규모 | 권장 모델 |
|---------|------|----------|
| 모놀리스 백엔드 | 소~중 | Pyramid |
| 모놀리스 백엔드 | 대 | Pyramid 또는 Diamond |
| 마이크로서비스 | 무관 | Diamond 또는 Honeycomb |
| 프론트엔드 SPA | 무관 | Trophy |
| UI 집약적 앱 | 무관 | Crab |

2024-2026 트렌드: Integration 테스트 비중 확대가 공통. "어떤 비율의 테스트를 작성할지 논쟁하는 것은 방해가 된다. 명확한 경계를 설정하고, 빠르고 안정적으로 실행되며, 유용한 이유로만 실패하는 표현력 있는 테스트를 작성하라." — Martin Fowler

## Test Double 전략

> Reference: [Software Engineering at Google Ch.13](https://abseil.io/resources/swe-book/html/ch13.html), [Martin Fowler - Mocks Aren't Stubs](https://martinfowler.com/articles/mocksArentStubs.html)

### 권장 우선순위

```
1순위: Real Implementation — 빠르고 결정적이면 항상 선호
2순위: Fake (경량 구현체) — 실제 구현이 불가능할 때 (예: in-memory DB)
3순위: Stub/Mock — Fake도 불가능할 때만 최후 수단
```

### 언제 무엇을 사용하는가

| Test Double | 사용 조건 | 예시 |
|-------------|----------|------|
| **Real** | 빠르고, 결정적이고, 외부 의존성 없음 | 도메인 로직, 유틸리티 |
| **Fake** | Real이 느리거나 비결정적 | in-memory DB, 로컬 파일 기반 스토리지 |
| **Stub** | 특정 응답을 반환해야 할 때 | 외부 API 응답 시뮬레이션 |
| **Mock** | 호출 여부 자체가 검증 대상일 때 | 이벤트 발행 확인, 알림 전송 확인 |

### Stubbing의 3가지 위험

1. **테스트 불명확** — 추가 setup 코드가 테스트 의도를 가린다
2. **테스트 취약** — 구현 세부사항 노출, 리팩토링 시 깨진다
3. **테스트 비효과** — stub된 함수가 실제와 동일하게 동작하는지 보장할 수 없다

### Interaction Testing 제한

Mock 호출 패턴 검증(몇 번 호출되었는지, 어떤 순서인지)은 **Change-detector test**를 만든다. 극히 제한적으로 사용:

- 허용: 캐싱이 DB 호출을 줄이는지, 이벤트가 발행되었는지
- 금지: 내부 메서드 호출 순서, 중간 단계 함수 호출 횟수

> "Mock 프레임워크의 과도한 사용은 실패했다. 작성은 쉬웠지만 지속적인 유지보수 노력이 필요했고 버그는 거의 발견하지 못했다." — Google

## Flaky Test 전략

> Reference: [Google Testing Blog - Flaky Tests](https://testing.googleblog.com/2016/05/flaky-tests-at-google-and-how-we.html), [Spotify - Test Flakiness](https://engineering.atspotify.com/2019/11/test-flakiness-methods-for-identifying-and-dealing-with-flaky-tests)

Flaky test의 진정한 비용은 **테스트 시스템에 대한 신뢰 상실**이다. Google에서 post-submit pass→fail 전환의 84%가 flaky test로 인한 것이었다.

### 3대 근본 원인 및 해결

| 원인 | 증상 | 해결 |
|------|------|------|
| **타이밍/비동기** | 간헐적 timeout, 순서 의존 | `sleep` 대신 predicate 기반 polling/이벤트 대기 |
| **테스트 순서 의존** | 단독 실행 시 통과, 전체 실행 시 실패 | 테스트 간 상태 리셋, 글로벌 상태 최소화 |
| **E2E 불안정** | 인프라/환경 이슈로 실패 | E2E 수를 최소화 ("500개 대신 5개") |

### 대응 플레이북

1. **실패 테스트만 재실행** — 전체 스위트 재실행 불필요
2. **3연속 실패 규칙** — 3번 연속 실패해야 실패로 보고
3. **자동 격리(Quarantine)** — 과도하게 flaky한 테스트를 자동 격리, 버그 티켓 생성
4. **대시보드 가시성** — Spotify는 대시보드 도입만으로 flaky 비율 6%→4% 감소 (기술적 변경 없이)

### 예방 원칙

- `sleep`/고정 지연 **절대 금지** — 이벤트 기반 대기만 사용
- 공유 가능 상태(shared mutable state) 금지
- 테스트 환경은 hermetic하게 격리
- 외부 서비스 호출은 Fake/Stub으로 대체

## 테스트 작성 시점

팀 역량과 프로젝트 성격에 따라 선택한다.

**기본값: Write-Tests-After + "PR에 테스트 포함 필수" 규칙**

- **TDD:** 설계 품질 향상, 진입장벽 높음 (~15-20% 채택). IBM/Microsoft 연합 연구에서 결함률 40-90% 감소 보고
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

### DAMP > DRY 원칙

> Reference: [Software Engineering at Google Ch.12](https://abseil.io/resources/swe-book/html/ch12.html)

프로덕션 코드는 DRY(중복 제거)를 우선하지만, **테스트 코드는 DAMP(Descriptive And Meaningful Phrases)를 우선**한다.

| 관점 | DRY | DAMP |
|------|-----|------|
| **우선순위** | 코드 중복 제거 | 각 테스트의 명확성 |
| **구조** | 공유 헬퍼/setup | 테스트 내에서 의도가 완결 |
| **장점** | 변경 시 한 곳만 수정 | 실패 시 원인 즉시 파악 |
| **위험** | 테스트 이해를 위해 여러 파일 추적 | 약간의 코드 중복 |

**기본값: DAMP** — 각 테스트는 독립적으로 읽을 수 있어야 한다.

안티패턴:
- setup에 숨겨진 복잡성 — 테스트를 이해하려면 beforeEach/beforeAll을 추적해야 함
- 범용 검증 헬퍼 — 하나의 함수에 여러 체크를 넣어 의도 불명확
- 과도한 테스트 유틸리티 추상화 — "테스트를 검증하기 위해 테스트가 필요하다면, 뭔가 잘못된 것이다" (Google)

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

## Property-Based Testing (선택)

> Reference: [OOPSLA 2025 - PBT Empirical Evaluation](https://dl.acm.org/doi/10.1145/3764068)

구체적 입력값 대신 **속성(property)**을 정의하고, 프레임워크가 수백~수천 개의 무작위 입력을 자동 생성하여 검증한다. OOPSLA 2025 실증 연구에서 일반 Unit Test 대비 **약 50배 많은 mutant**를 발견.

### 언제 사용하는가

| 패턴 | 예시 |
|------|------|
| **Roundtrip** (encode → decode = original) | JSON serialize/deserialize, compress/decompress |
| **수학적 속성** (교환법칙, 결합법칙, 멱등성) | sort 안정성, merge 연산 |
| **불변 조건** (항상 참인 조건) | 필터 결과 길이 ≤ 원본 길이, 파싱 결과가 항상 유효 |
| **Oracle 비교** (단순 구현 vs 최적화 구현) | 최적화된 알고리즘과 brute-force 비교 |

### 도구

| 언어 | 도구 |
|------|------|
| Python | [Hypothesis](https://hypothesis.readthedocs.io/) |
| JS/TS | [fast-check](https://github.com/dubzzz/fast-check) |
| Haskell/Erlang | QuickCheck |

**기본값: 선택 사항.** 비즈니스 로직의 불변 속성이 명확한 경우 도입 검토.

## Contract Testing (마이크로서비스 권장)

> Reference: [Pact Documentation](https://docs.pact.io/), [Martin Fowler - Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)

서비스 간 인터페이스 계약을 코드로 검증하는 **Consumer-Driven Contracts (CDC)** 방식.

### CDC 워크플로우

1. **Consumer**가 기대하는 요청/응답을 정의
2. Consumer 테스트 실행 중 **Contract 파일** 자동 생성
3. **Provider**가 Contract 파일 대비 검증 테스트 실행
4. Pact Broker에 계약 저장 → CI/CD에서 지속적 호환성 검증

### 장점

- E2E 의존성 감소 — Contract 테스트는 **분 단위**, E2E는 시간 단위
- 실제 사용되는 통신 경로만 테스트 — 미사용 Provider 동작은 자유롭게 변경 가능
- 서비스 **독립 배포** 가능 — 호환성이 계약으로 보장

### vs OpenAPI/Schema Validation

| 방식 | 검증 대상 | 특징 |
|------|----------|------|
| **OpenAPI** | 가능한 모든 상태 | 스키마 정합성, 문서화 용도 |
| **Pact (CDC)** | 실제 사용되는 통신 | 실용적, 소비자 관점 |

도구: [Pact](https://docs.pact.io/) (가장 성숙, 다국어 지원)

**기본값: 마이크로서비스 아키텍처에서 권장.** 모놀리스에서는 불필요.

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
| Over-DRY setup (과도한 beforeEach 추상화) | 테스트 이해를 위해 여러 파일 추적 필요 |
| Mock 호출 패턴(순서/횟수) 과다 검증 | Change-detector test 생성, 리팩토링마다 깨짐 |
| 테스트 내 조건문/반복문 사용 | 테스트 자체의 정확성 검증 불가 |

---

## AI 생성 코드 테스팅

> Reference: [testkube.io - Testing AI-Generated Code](https://testkube.io/blog/testing-ai-generated-code), [ThoughtWorks Technology Radar Vol.32-33](https://www.thoughtworks.com/radar)

AI 생성 코드는 인간 작성 코드 대비 **1.7배 더 많은 주요 이슈**, **2.74배 더 많은 보안 취약점**을 포함한다 (2025 분석).

### 3대 위험

| 위험 | 설명 |
|------|------|
| **Logic Drift** | 구문적으로 올바르지만 비가시적 비즈니스 규칙 위반. 주석/암묵적 지식에만 존재하는 제약 무시 |
| **Dependency Mismatch** | 2-3년 전 학습 데이터 기반 — 현재 보안 패치 미반영, CVE 갭 |
| **Regression Roulette** | 하류 의존성을 인식하지 못하는 최적화 — 드물게 실행되는 코드에서 문제 발현 |

### 대응 전략

| 전략 | 설명 |
|------|------|
| **비즈니스 불변식 테스트** | 출력 정확성뿐 아니라 비즈니스 규칙 검증 (예: `discount_total <= order_total`) |
| **AI 코드 라벨링** | PR에 `[AI-Generated]` 태그 → 리뷰 시 다른 마인드셋 유도 |
| **의존성 스캐닝 강화** | Snyk, OWASP Dependency-Check 등 자동 통합 |
| **분기별 AI 코드 감사** | AI가 접촉한 프로덕션 코드 정기 감사 |

> AI 시대에 TDD와 Static Analysis 등 **기존 테스팅 관행의 강화**가 더 중요해진다. — ThoughtWorks Radar

참조: `/docs/ai-assisted-development/`

## Testing in Production (선택)

> Reference: [Netflix - Testing in Production](https://launchdarkly.com/blog/testing-in-production-the-netflix-way/), [Martin Fowler - Synthetic Monitoring](https://martinfowler.com/bliki/SyntheticMonitoring.html)

스테이징 환경만으로는 프로덕션의 모든 조건을 재현할 수 없다. Shift-Left(조기 테스트) + Shift-Right(프로덕션 검증) = **Continuous Testing**.

### 5가지 기법

| 기법 | 설명 | 위험도 |
|------|------|--------|
| **Synthetic Monitoring** | 프로덕션에서 자동화된 테스트 서브셋을 주기적 실행. 가용성 실패 선제 탐지 | 낮음 |
| **Feature Flags** | 기능 노출을 코드 배포와 분리. 즉시 롤백 가능 | 낮음 |
| **Canary Releases** | 실 트래픽의 2-10%를 새 버전으로 라우팅하여 검증 | 중간 |
| **Dark Launches** | 실 데이터에서 백엔드 동작 검증, 사용자에게는 미노출 | 중간 |
| **Chaos Engineering** | 프로덕션 인스턴스를 의도적으로 교란하여 복원력 검증 | 높음 |

**기본값: Synthetic Monitoring부터 시작.** Canary/Chaos는 서비스 규모와 운영 성숙도가 충분할 때 도입.

참조: `/docs/ci-cd/` (배포 전략), `/docs/observability/` (모니터링)

## 테스트 선택/우선순위 (선택)

> Reference: [Shopify - Spark Joy by Running Fewer Tests](https://shopify.engineering/spark-joy-by-running-fewer-tests), [Shopify - Test Budget](https://shopify.engineering/test-budget-time-constrained-ci-feedback)

테스트 스위트가 커지면 전체 실행 시간이 CI 병목이 된다.

### Dynamic Analysis 기반 Test Selection

테스트 실행 중 커버리지를 매핑하고, 코드 변경에 영향받는 테스트만 선택적으로 실행한다.

Shopify 결과: **60% 테스트 선택으로 99.94% 재현율** (8,360 커밋 중 5건 누락)

### Test Budget

테스트 실행에 **고정 시간 예산**을 할당하여 속도와 위험 사이 균형을 잡는다.

- **우선순위 전략**: 실패율 기반(과거 빈도) > 테스트 지속시간(빠른 것 우선)
- 스위트의 **70% 실행만으로 대부분의 실패를 포착**

**기본값: 전체 실행.** 테스트 스위트가 30분 이상이면 Test Selection 검토.

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
- [ ] Test Double은 Real > Fake > Mock 우선순위 준수
- [ ] AI 생성 코드인 경우 비즈니스 불변식 테스트 포함
