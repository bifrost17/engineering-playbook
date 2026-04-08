# 업계 리더 및 기업 테스팅 가이드 리서치

> 작성일: 2026-04-08
> 목적: Engineering Playbook 테스팅 가이드 보강을 위한 업계 베스트 프랙티스 리서치

---

## 1. Martin Fowler / ThoughtWorks

### 1.1 The Practical Test Pyramid

- **URL:** https://martinfowler.com/articles/practical-test-pyramid.html
- **추가 참고:** https://martinfowler.com/testing/

**핵심 철학:**
자동화된 테스트 스위트를 단일 명령으로 실행하여 "green"이면 프로덕션 배포에 자신감을 갖는 **Self-Testing Code** 개념. 테스트 자동화의 진정한 가치는 높은 커버리지 수치가 아닌, 규율 있고 사려 깊은 구현에서 나온다.

**핵심 실천 사항:**

| 원칙 | 설명 |
|------|------|
| **피라미드 비율 유지** | 많은 Unit, 일부 Integration, 극소수 E2E |
| **Ice-Cream Cone 안티패턴 회피** | 고수준 테스트가 저수준보다 많은 역피라미드 금지 |
| **테스트를 가능한 낮은 레벨로** | 상위 테스트에서 버그 발견 시, 하위 테스트 추가 후 상위 중복 테스트 삭제 |
| **용어 합의 필수** | 업계 표준 용어가 없으므로 팀 내 정의를 명확히 |
| **파이프라인 단계별 배치** | 빠른 Unit → 중간 Integration → 최종 E2E 순서로 CI 파이프라인 구성 |

**Test Double 가이드라인:**

- **Solitary vs Sociable 단위 테스트**: 모든 협력 객체를 mock하는 방식과 실제 객체를 허용하는 방식 모두 유효
- 외부 의존성(DB, 네트워크)은 Stub 처리하되, 내부 협력 객체는 실제 사용 고려
- 어느 쪽이든 "어색함을 줄이고 자신감을 유지하는 방향"으로 선택

**Contract Testing 강조:**

- Consumer-Driven Contracts(CDC)로 서비스 간 인터페이스 계약 검증
- Pact 같은 도구로 양쪽 팀이 테스트할 수 있는 아티팩트 생성
- 서비스 경계에서 breaking change 방지

**고유한 인사이트:**

- **테스트 코드는 프로덕션 코드와 동일한 엄격함으로 관리** — 하지만 가독성을 위해 일부 중복은 허용 (Rule of Three)
- **Private 메서드 테스트 = 설계 문제 신호** — 리팩토링으로 해결
- **Test Cancer 경고**: 유지보수되지 않는 테스트 스위트는 자산이 아니라 부채
- **Subcutaneous Testing**: UI 바로 아래 레이어에서 테스트하여 GUI 취약성 회피
- **Synthetic Monitoring**: 프로덕션에서 자동화된 테스트 서브셋을 주기적으로 실행
- **Test Impact Analysis**: 콜그래프 분석으로 코드 변경에 영향받는 테스트만 실행

### 1.2 Mocks Aren't Stubs

- **URL:** https://martinfowler.com/articles/mocksArentStubs.html

**핵심 분석: Classical vs Mockist TDD**

| 관점 | Classical (Fowler 선호) | Mockist |
|------|------------------------|---------|
| **검증 방식** | State Verification (결과 상태 확인) | Behavior Verification (호출 확인) |
| **테스트 대상** | 실제 객체 사용, 불편한 것만 대체 | 관심 있는 동작의 모든 객체 mock |
| **격리** | 미니 통합 테스트 역할 | 완전 격리 |
| **리팩토링 영향** | 최종 상태만 확인하므로 강건함 | 호출 패턴 변경 시 테스트 깨짐 |
| **설계 영향** | Middle-out 개발, 도메인 모델 우선 | Outside-in 개발, 인터페이스 우선 |

**Fowler의 결론:**
- Classical TDD를 선호하지만 양쪽 모두 장단점 존재
- **핵심 경고**: 두 방식 모두 coarse-grained 수락 테스트가 반드시 필요 — 단위 테스트만으로는 시스템 수준 통합 실패를 잡을 수 없음

---

## 2. Google (Software Engineering at Google)

### 2.1 Unit Testing (Chapter 12)

- **URL:** https://abseil.io/resources/swe-book/html/ch12.html

**핵심 철학:**
**유지보수성(Maintainability)**이 최우선 목표. "작성 후 실패할 때까지 다시 생각할 필요 없고, 실패하면 실제 버그를 명확한 원인과 함께 알려주는 테스트"가 이상적.

**핵심 실천 사항:**

| 원칙 | 설명 |
|------|------|
| **Test via Public APIs** | 사용자가 사용하는 방식으로 테스트. 구현 세부사항이 아닌 공개 API 호출 |
| **Test State, Not Interactions** | 시스템이 무엇을 생산하는지 검증, 어떻게 생산하는지 검증하지 않음 |
| **Beyonce Rule** | "네가 좋아했으면 테스트를 걸어둬야지" — 관심 있는 모든 것에 테스트 작성 |
| **DAMP > DRY** | Descriptive And Meaningful Phrases 우선. 테스트 코드에서 약간의 중복은 명확성을 위해 허용 |
| **Unchanging Tests** | 리팩토링, 기능 추가, 버그 수정 시 테스트가 변경되지 않아야 함 |
| **Behavior-Driven 구조** | 메서드별이 아닌 행위별로 테스트 조직 (given/when/then) |
| **테스트에서 로직 제거** | 조건문, 반복문 금지. 한눈에 정확성 확인 가능해야 함 |
| **강력한 실패 메시지** | 기대값 vs 실제값을 맥락 정보와 함께 명확하게 표시 |

**테스트 비율:** Unit 80% / Broader-scoped 20%

**안티패턴:**
1. 메서드 단위 테스트 — 관련 없는 행위를 하나로 묶는 비대한 테스트 생성
2. 상호작용 mock — 구현을 테스트하게 됨
3. Setup에 숨겨진 복잡성 — 테스트 완전성 불투명
4. 범용 검증 헬퍼 — 하나의 메서드에 여러 체크를 넣어 명확성 저하
5. 과도한 헬퍼 추상화 — Over-DRY로 테스트 이해 불가

### 2.2 Test Doubles (Chapter 13)

- **URL:** https://abseil.io/resources/swe-book/html/ch13.html

**Google의 핵심 교훈: Mock 프레임워크의 과도한 사용은 실패했다**

> "처음 사용했을 때는 이상적으로 보였지만, 수년 후 그 대가를 깨달았다: 작성은 쉬웠지만 지속적인 유지보수 노력이 필요했고 버그는 거의 발견하지 못했다."

**권장 우선순위:**

```
1순위: Real Implementation (실제 구현체) — 빠르고 결정적이면 항상 선호
2순위: Fake (경량 구현체) — 실제 구현이 불가능할 때
3순위: Stub/Mock — Fake도 불가능할 때만 최후 수단
```

**Fake 운영 원칙:**
- **실제 구현체 소유 팀이 Fake도 작성/유지** — 괴리 방지
- Fake에는 자체 **계약 테스트(Contract Test)** 필요 — 실제/Fake에 동일 테스트 스위트 실행
- **Fidelity(충실도)** 개념: 동일 입력에 동일 출력과 상태 변화를 보장

**Stubbing의 3가지 위험:**
1. **테스트 불명확** — 추가 Setup 코드가 의도를 가림
2. **테스트 취약** — 구현 세부사항 노출, 코드 변경 시 깨짐
3. **테스트 비효과** — 스텁된 함수가 실제와 동일하게 동작하는지 검증 불가

**Interaction Testing:** "Change-detector tests"를 만들기 때문에 극히 제한적으로 사용. 캐싱이 DB 호출을 줄이는지 확인하는 등의 특수한 경우에만 허용.

**`@DoNotMock` 어노테이션:** API 소유자가 mock 금지를 선언하여 엔지니어를 더 나은 대안(실제 구현 또는 Fake)으로 유도.

### 2.3 Larger Testing (Chapter 14)

- **URL:** https://abseil.io/resources/swe-book/html/ch14.html

**테스트 크기(Size) 프레임워크:**

| 크기 | 제약 |
|------|------|
| **Small** | 단일 스레드, 단일 프로세스, 단일 머신 |
| **Medium** | 단일 머신, 여러 프로세스 |
| **Large** | 여러 머신, 네트워크 접근 가능 |

**Larger Test가 필요한 이유 (Unit Test가 못 잡는 것들):**
1. **불충실한 Test Double** — Mock이 실제 의존성 동작을 잘못 표현
2. **설정 문제** — Google에서 "주요 장애의 1위 원인"
3. **부하/스트레스 조건** — 현실적인 배포 토폴로지 필요
4. **예측 못한 행동** — 사용자가 발견하는 이슈는 대부분 Unit Test 설계 시 예상 못한 것
5. **창발적 시스템 속성** — 서비스 간 상호작용에서 발생하는 행동

**A/B Diff Testing:** Google에서 가장 흔한 대규모 테스트. 구/신 버전에 동일 트래픽 전송 후 응답 비교.

**Record/Replay 패턴:** 대규모 테스트에서 외부 서비스 트래픽을 Record Mode로 캡처 → 개발 시 Replay Mode로 재생. 의존성 취약성을 줄이면서 충실도 유지.

**핵심 교훈:**
- 시스템 성장 시 시나리오 조합은 기하급수적으로 증가 → **Chained Tests**로 관리 (한 테스트의 출력이 다른 테스트의 입력)
- **명확한 소유권** 필수 — 대규모 테스트의 건강과 유지보수 책임자를 문서화
- Sleep 기반 대기를 **이벤트 기반 대기**(polling, handler, notification)로 교체

### 2.4 Flaky Tests at Google

- **URL:** https://testing.googleblog.com/2016/05/flaky-tests-at-google-and-how-we.html

**규모:**
- 전체 테스트 실행의 **1.5%가 flaky**
- 전체 테스트의 **약 16%가 일부 flakiness 보임**
- Post-submit 테스트의 pass→fail 전환 중 **84%가 flaky 테스트**

**대응 전략:**

| 전략 | 설명 |
|------|------|
| **실패 테스트만 재실행** | 개발자가 실패한 테스트만 선택적 재실행 |
| **자동 재시도** | 실패 시 자동으로 다시 실행 |
| **3연속 실패 규칙** | 3번 연속 실패해야 실패로 보고 |
| **자동 격리(Quarantine)** | 과도하게 flaky한 테스트를 자동 격리, 버그 파일 |
| **Flakiness 변화 감지** | 코드 제출 후 flakiness 수준 변화를 자동 탐지 |

---

## 3. Kent C. Dodds — Testing Trophy

### 3.1 Write Tests. Not Too Many. Mostly Integration.

- **URL:** https://kentcdodds.com/blog/write-tests
- **추가 참고:** https://kentcdodds.com/blog/static-vs-unit-vs-integration-vs-e2e-tests

**핵심 철학:**
"테스트가 소프트웨어가 사용되는 방식과 유사할수록, 더 많은 자신감을 줄 수 있다."

**Testing Trophy 구조:**

```
     E2E         ← 최고 자신감, 최고 비용
   Integration   ← 최적 ROI (주력)
     Unit        ← 순수 함수, 엣지 케이스
   Static        ← TypeScript, ESLint (기반)
```

**3가지 핵심 원칙:**

| 원칙 | 설명 |
|------|------|
| **Write Tests** | 로컬에서 버그를 잡는 것이 프로덕션에서 잡는 것보다 시간 절약 |
| **Not Too Many** | 70% 이상에서 수익 체감. 100% 추구는 역효과 |
| **Mostly Integration** | Integration이 최적 ROI — 컴포넌트가 함께 동작하는지 검증 |

**Mock 사용 경고:**
> "무언가를 mock하면 테스트 대상과 mock된 대상 사이의 통합에 대한 모든 자신감을 제거하는 것이다."

**코드 커버리지 관점:**
- 70% 이상에서 수익 체감
- 오픈소스 라이브러리는 예외 — 많은 프로젝트에 영향을 미치므로 거의 100% 유지
- 커버리지 수치보다 "이 테스트가 변경 사항을 배포하는 자신감을 높이는가?"가 중요

**고유한 인사이트:**
- React에서 Shallow Rendering 반대 — 실제 컴포넌트 상호작용 테스트 권장
- Static Analysis를 Testing Trophy의 기반 레이어로 포함 — 런타임 테스트 이전에 결함 차단

---

## 4. Microsoft Engineering Fundamentals

### 4.1 Unit Testing Best Practices

- **URL:** https://learn.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices
- **추가 참고:** https://microsoft.github.io/code-with-engineering-playbook/automated-testing/unit-testing/

**핵심 실천 사항:**

| 원칙 | 설명 |
|------|------|
| **100% 신뢰성** | 테스트 실패 = 코드 버그 (환경/인프라 아님) |
| **밀리초 단위 실행** | 전체 Unit Test 스위트가 수초 이내 |
| **외부 의존성 완전 제거** | 인프라 의존은 Integration Test에서만 |
| **AAA 패턴** | Arrange/Act/Assert 구조로 가독성 보장 |

**네이밍 규칙:** `UnitName_StateUnderTest_ExpectedResult`

**명시적 금지 사항:**
- `Sleep` 사용 (상태 통신의 대안을 찾을 것)
- 디스크 I/O (외부 의존성 생성)
- 서드파티 API 직접 호출 (인터페이스 뒤에 추상화)

### 4.2 PACT Contract Testing 권장

- **URL:** https://devblogs.microsoft.com/ise/pact-contract-testing-because-not-everything-needs-full-integration-tests/

Microsoft ISE(Industry Solutions Engineering)에서 Pact 기반 Contract Testing을 공식 권장. "모든 것이 전체 통합 테스트를 필요로 하지는 않는다"는 원칙.

### 4.3 Performance Testing

- **URL:** https://devblogs.microsoft.com/ise/performance-testing/

- 전용 성능 환경 필요 — 공유 환경은 부정확한 결과 유발
- 빈번하고 이른 테스트로 성능 이슈 조기 발견
- 개발자 로컬 환경에서 경량 성능 테스트 수행 권장 ("Shift-Left")

### 4.4 Chaos Engineering / Reliability Testing

- **URL:** https://learn.microsoft.com/en-us/power-platform/well-architected/reliability/testing-strategy

- Fault Injection으로 실제 시나리오 시뮬레이션
- 프로덕션에 영향 미치기 전에 사전 발견 및 수정
- DevSecOps의 Shift-Left 원칙과 통합

---

## 5. Spotify Engineering

### 5.1 Flaky Test 대응 전략

- **URL:** https://engineering.atspotify.com/2019/11/test-flakiness-methods-for-identifying-and-dealing-with-flaky-tests

**핵심 통찰:** "Flaky test의 진정한 비용은 테스트에 대한 자신감 상실이다."

**3가지 근본 원인 및 해결:**

| 원인 | 해결 |
|------|------|
| **일관성 없는 Assertion 타이밍** | Predicate로 안정 상태까지 Polling 후 Assert |
| **테스트 순서 의존성** | 테스트 간 상태 리셋, 글로벌 상태 최소화 |
| **E2E 테스트 불안정** | "500개 대신 5개만 작성" |

**도구:**

| 도구 | 역할 |
|------|------|
| **Odeneye** | 테스트 스위트 시각화 — 산발적 실패(flaky) vs 일괄 실패(인프라) 구분 |
| **Flakybot** | PR에서 테스트 flakiness 사전 검증 후 머지 |
| **대시보드** | 가시성만으로 6% → 4% 감소 (2개월, 기술적 변경 없이) |

### 5.2 테스트 전략 관련 추가 인사이트

- **도구 제한 전략**: Spotify는 에이전트에게 코드 검색/문서 도구를 의도적으로 제외하고 verify/git/bash만 제공 — "비예측성의 차원"을 줄여 예측 가능성 향상
- **실험 품질 우선**: 실험 속도 향상에서 실험 품질 극대화로 전환 (2025)

---

## 6. Shopify Engineering

### 6.1 Spark Joy by Running Fewer Tests

- **URL:** https://shopify.engineering/spark-joy-by-running-fewer-tests

**핵심 문제:** Ruby 모놀리스에 150,000+ 테스트, 연간 20-30% 성장, 실행에 30-40분

**Dynamic Analysis 기반 Test Selection:**
- 테스트 실행 중 메서드 호출을 기록하여 각 테스트가 커버하는 파일 매핑
- 코드 변경 사항 추적으로 영향받는 테스트만 식별/실행

**결과:**

| 지표 | 수치 |
|------|------|
| 재현율(Recall) | 99.94% (8,360 커밋 중 5건 누락) |
| 선택율 | 약 60% 테스트 선택 |
| 고영향 빌드 | 40%의 빌드가 전체 테스트의 20% 미만 실행 |
| 컴퓨팅 절약 | 25% 인프라 시간 감소 |

**논쟁적이지만 효과적인 관행:**
- 지속적으로 실패하는 테스트를 **자동 비활성화** — 머지된 PR의 0.06%만 실패 포함
- 개발자의 2% 미만만 전체 테스트 스위트 실행 요청 → 선택적 접근에 높은 신뢰

### 6.2 Test Budget: Time-Constrained CI Feedback

- **URL:** https://shopify.engineering/test-budget-time-constrained-ci-feedback

**핵심 개념:** 테스트 실행에 고정 시간 예산을 할당하여 속도와 위험 사이 균형

**6가지 우선순위 전략 평가:**

| 전략 | 효과 |
|------|------|
| 실패율 기반 (과거 빈도) | 약간 우수 |
| 테스트 지속시간 (빠른 것 우선) | 보통 |
| 코드 변동률 (자주 변경 파일) | 비효과적 |
| 커버리지 지표 | 보통 |
| 복잡도 (LOC) | 비효과적 |
| 랜덤 | 기준선 |

**핵심 발견:** Test Selection 스위트의 **70%만 실행**해도 대부분의 실패를 포착.

---

## 7. Netflix Engineering

### 7.1 Chaos Engineering & Testing in Production

- **참고:** https://newsletter.systemdesign.one/p/chaos-engineering
- **참고:** https://launchdarkly.com/blog/testing-in-production-the-netflix-way/

**핵심 철학:** 테스팅은 스테이징에서 끝나지 않는다. 프로덕션에서도 지속적으로 검증한다.

**도구 및 방법론:**

| 도구/방법 | 설명 |
|-----------|------|
| **Chaos Monkey** | 프로덕션 인스턴스를 무작위로 종료하여 복원력 검증 |
| **ChAP (Chaos Automation Platform)** | 배포 플로우에 통합된 Chaos 실험 자동화 |
| **Canary 배포** | 실 트래픽의 2-10%를 새 버전으로 라우팅하여 검증 |
| **Blast Radius 제한** | 최대 5% 트래픽만 chaos에 노출 |

**Canary Analysis 프로세스:**
1. Control(안정 버전)과 Experiment(새 버전)에 동일 비율 트래픽 라우팅
2. "Canary가 의미 있게 더 나쁜가?"를 자동 분석
3. 임계값 초과 시 자동 롤백, 통과 시 점진적 트래픽 증가

**Testing in Production 5가지 기법:**
1. **Canary Releases** — 실 부하에서 새 코드 검증
2. **Feature Flags** — 즉시 롤백 가능한 기능 노출 제어
3. **Dark Launches** — 실 데이터에서 백엔드 동작 검증 (사용자 미노출)
4. **Synthetic Monitoring** — 가용성 실패를 선제적 탐지
5. **Chaos Engineering** — 복원력 테스트

---

## 8. Pact — Contract Testing

- **URL:** https://docs.pact.io/
- **Microsoft 참고:** https://devblogs.microsoft.com/ise/pact-contract-testing-because-not-everything-needs-full-integration-tests/

**핵심 개념:**
HTTP 및 메시지 통합을 계약 테스트로 검증하는 코드 우선 도구. 서비스 간 메시지가 문서화된 계약에 부합하는지 확인.

**Consumer-Driven Contracts 워크플로우:**

```
1. Consumer가 기대하는 요청/응답 정의
2. Consumer 자동화 테스트 실행 중 계약(Contract) 파일 생성
3. Provider가 계약 파일 대비 검증 테스트 실행
4. Pact Broker에 계약 저장 → CI/CD에서 지속적 호환성 검증
```

**장점:**
- 느리고 취약한 E2E 테스트 의존성 감소
- 서비스 상호작용을 격리된 환경에서 검증 가능
- 계약 테스트는 분 단위 실행 (E2E는 시간 단위)
- 실제 사용되는 통신 경로만 테스트 — 미사용 Provider 동작은 자유롭게 변경 가능

**vs 정적 스키마 (OpenAPI 등):**
- OpenAPI: 가능한 모든 상태 기술
- Pact: 실제 사용되는 통신만 테스트 → 더 실용적

---

## 9. 테스트 전략 모델 비교 (web.dev)

- **URL:** https://web.dev/articles/ta-strategies

| 모델 | 구조 | 적합한 상황 |
|------|------|------------|
| **Test Pyramid** | Unit(多) → Integration → E2E(少) | 기본 출발점, 대부분의 프로젝트 |
| **Test Diamond** | Unit(축소) → Integration(확대) → E2E | Unit 100% 커버리지가 유지보수 부담인 경우 |
| **Testing Honeycomb** | Implementation Details → Integration(主) → Integrated | 마이크로서비스 (서비스 간 상호작용이 핵심) |
| **Testing Trophy** | Static → Unit → Integration(主) → E2E | 현대적 개발 (TypeScript, ESLint 활용) |
| **Testing Crab** | Unit(최소) → Component/API → E2E+Visual(主) | UI 중심 앱, 시각적 일관성 중요 |

**전략 선택 기준:**

| 요소 | 권장 |
|------|------|
| **앱 크기** | 소규모 → Crab, 대규모 → Pyramid 변형 |
| **팀 구성** | 개발자 전용 → Pyramid/Trophy, Dev+QA 혼합 → Crab |
| **아키텍처** | 마이크로서비스 → Honeycomb, 모놀리스 → Pyramid/Trophy, UI 중심 → Crab |
| **수동 테스트 의존도** | 높음 → Crab, 낮음 → Pyramid/Trophy/Diamond |

**핵심 원칙:** "It depends." — 임의의 커버리지 지표가 아닌, 사용자의 실제 필요를 충족하는 전략을 선택.

---

## 10. Property-Based Testing & Mutation Testing (최신 연구)

### 10.1 Property-Based Testing

- **URL:** https://dl.acm.org/doi/10.1145/3764068 (OOPSLA 2025, UC San Diego)

**핵심 발견 (2025 실증 연구):**
- 평균적으로 각 PBT(Property-Based Test)는 일반 Unit Test 대비 **약 50배 많은 mutant**를 발견
- 예외를 찾는 테스트, 컬렉션 포함 테스트, 타입 확인 테스트가 다른 PBT 대비 **19배 이상 효과적**

**도구:** Hypothesis (Python), fast-check (JS/TS), QuickCheck (Haskell 계열)

### 10.2 Mutation Testing (2025-2026)

- **URL:** https://mastersoftwaretesting.com/testing-fundamentals/types-of-testing/mutation-testing

**최신 동향:**
- 2026 CI/CD 과제: Kubernetes에서 병렬 실행과 파일당 mutant 제한으로 리소스 관리
- **Mutation Score 80%** 이상이 2026년 표준 목표 (AI/ML 신뢰성)
- 라인 커버리지 대비 **2배 런타임 비용**이지만, 프로덕션 버그율 **55% 감소** (2025 DevOps 설문)
- Stryker .NET이 Python 도구 대비 **2배 빠른 mutant 생성**, 12% 높은 정밀도

---

## 11. AI 생성 코드의 테스팅 (2025-2026)

### 11.1 AI 생성 코드의 위험

- **URL:** https://testkube.io/blog/testing-ai-generated-code

**3가지 핵심 위험:**

| 위험 | 설명 |
|------|------|
| **Logic Drift** | 구문적으로 올바르지만 비가시적 비즈니스 규칙 위반. 주석이나 암묵적 지식에만 존재하는 제약 무시 |
| **Dependency Mismatch** | AI 추천이 2-3년 전 학습 데이터 기반 — 현재 보안 패치 미반영, CVE 갭 |
| **Regression Roulette** | 최적화가 하류 의존성을 인식하지 못함 — 드물게 실행되는 코드에서만 문제 발견 |

**통계:**
- AI 공동 작성 코드는 인간 작성 코드 대비 **1.7배 더 많은 주요 이슈** 포함 (2025.12 분석)
- **2.74배 더 많은 보안 취약점**, 75% 더 많은 잘못된 설정
- Copilot 생성 코드는 인간 작성 대비 **40% 더 많은 보안 결함**

### 11.2 권장 대응 전략

| 전략 | 설명 |
|------|------|
| **비즈니스 불변식 테스트** | 출력 정확성뿐 아니라 비즈니스 규칙 검증 (예: `discount_total <= order_total`) |
| **AI 코드 라벨링** | PR에 `[AI-Generated]` 태그 → 다른 리뷰 마인드셋 유도 |
| **프로덕션 유사 데이터 테스트** | 엣지 케이스 노출을 위해 실제와 유사한 데이터 사용 |
| **Side-by-side 실행** | AI 생성 코드와 원본 코드를 나란히 실행 비교 |
| **의존성 스캐닝** | Snyk, OWASP Dependency-Check 자동 통합 |
| **버전 고정** | 범위 지정 대신 정확한 의존성 버전 Pin |
| **분기별 AI 코드 감사** | AI가 접촉한 프로덕션 코드 정기 감사 |

### 11.3 ThoughtWorks Technology Radar 관점 (Vol.32-33, 2025)

- **URL:** https://www.thoughtworks.com/radar

- Microsoft 연구: AI 기반 자신감이 비판적 사고를 대체하는 위험
- 코딩 에이전트가 더 큰 변경 세트를 생성 → 리뷰 어려움 증가
- **TDD와 Static Analysis 등 기존 관행 강화** 권장
- AI에 전체 업그레이드 위임 대신, 작고 검증 가능한 단계로 분할: 컴파일 오류 분석 → 마이그레이션 diff 생성 → 테스트 반복 검증

---

## 12. Shift-Left Testing & Continuous Testing

- **참고:** https://www.browserstack.com/guide/devops-shift-left-testing
- **참고:** https://www.ibm.com/think/topics/shift-left-testing

**핵심 개념:**
테스팅을 SDLC 초기로 이동. 프로덕션에서 버그를 수정하는 비용은 초기 발견 대비 **100배**.

**2025-2026 트렌드:**
- DevSecOps: 보안 테스트도 Shift-Left
- "Test as You Code" 철학: 개발자가 개발 프로세스의 일부로 Unit, Integration, Performance 테스트 작성
- AI/ML을 테스트 라이프사이클에 내장: 테스트 케이스 생성, 예측 분석, 지능형 버그 분류

**Shift-Left + Shift-Right 통합:**
- Shift-Left: 조기 결함 발견 (Static Analysis, Unit/Integration Test, Security Scan)
- Shift-Right: 프로덕션 모니터링, Canary 배포, Chaos Engineering, Synthetic Monitoring
- 양쪽을 결합한 **Continuous Testing** 전략이 가장 효과적

---

## 공통 원칙 (Common Principles across Industry)

모든 주요 출처에서 공통적으로 강조하는 원칙:

### 1. Integration 테스트 비중 확대
- **Google**: Unit 80% / Broader 20%, 하지만 Mock 과다사용 경계
- **Kent C. Dodds**: "Mostly Integration" — Integration이 최적 ROI
- **Martin Fowler**: Test Pyramid은 여전히 유효하나, 현대적 관점에서 과도하게 단순화
- **Spotify Honeycomb**: 마이크로서비스에서 Integration이 가장 중요
- **공통 결론**: 2024-2026 업계 트렌드는 **Integration 테스트 비중 확대**

### 2. Mock/Stub 사용 최소화
- **Google**: Mock 프레임워크 과다 사용의 실패를 경험. Real Implementation > Fake > Mock 순서 권장
- **Kent C. Dodds**: Mock은 통합에 대한 자신감을 제거
- **Martin Fowler**: State Verification(Classical)이 Behavior Verification(Mockist)보다 덜 취약
- **공통 결론**: **외부 의존성만 Mock, 내부는 실제 구현 사용 선호**

### 3. 테스트는 Public API를 통해
- **Google**: "사용자가 사용하는 방식으로 테스트"
- **Kent C. Dodds**: "테스트가 소프트웨어가 사용되는 방식과 유사할수록"
- **Martin Fowler**: 구현 세부사항이 아닌 관찰 가능한 행동 테스트
- **공통 결론**: **구현이 아닌 행동을 테스트**

### 4. Flaky Test 즉시 대응
- **Google**: 자동 격리 + 3연속 실패 규칙 + 전담 팀
- **Spotify**: 시각화 대시보드만으로 6%→4% 감소 + Flakybot 사전 검증
- **공통 결론**: **가시성 확보가 첫 번째, 자동화된 격리가 두 번째**

### 5. 테스트 코드 품질 = 프로덕션 코드 품질
- **Google**: DAMP > DRY, 하지만 테스트 인프라에는 자체 테스트 필요
- **Martin Fowler**: Rule of Three 적용, 테스트 코드도 동일한 엄격함
- **Microsoft**: AAA 패턴, 일관된 네이밍
- **공통 결론**: **테스트 코드를 일급 시민으로 대우**

### 6. Contract Testing for Microservices
- **Martin Fowler**: Consumer-Driven Contracts 권장
- **Microsoft**: Pact 기반 Contract Testing 공식 채택
- **Pact**: 코드 우선, 실제 사용 경로만 테스트
- **공통 결론**: **마이크로서비스 아키텍처에서 필수**

---

## 최신 트렌드 (Modern Trends in Testing, 2025-2026)

### 1. AI 생성 코드에 대한 테스팅 강화
- AI 코드의 보안 취약점 2.74배 증가 → 테스트 강화 필수
- 비즈니스 불변식 테스트, AI 코드 라벨링, 분기별 감사
- ThoughtWorks: TDD와 Static Analysis 등 **기존 관행 강화가 AI 시대에 더 중요**

### 2. Testing in Production
- Netflix Chaos Engineering, Canary 배포가 업계 표준으로 자리잡음
- Synthetic Monitoring으로 프로덕션에서 자동화된 테스트 실행
- Shift-Left + Shift-Right의 **Continuous Testing** 통합

### 3. Property-Based Testing 부상
- 일반 Unit Test 대비 50배 효과적 (mutant 발견 기준)
- Hypothesis, fast-check 등 도구 성숙
- Mutation Testing과 결합 시 시너지 극대화

### 4. 테스트 선택/우선순위 최적화
- Shopify: Dynamic Analysis로 60% 테스트만 선택, 99.94% 재현율
- Shopify: Test Budget 개념 — 70% 실행으로 대부분 실패 포착
- Google: Test Impact Analysis — 콜그래프 기반 영향 테스트 식별
- **핵심**: 전체 테스트 실행에서 **지능형 선택적 실행**으로 전환

### 5. 테스트 전략 모델 다양화
- 단일 피라미드에서 Trophy, Diamond, Honeycomb, Crab 등 다양한 모델로 확장
- 프로젝트 특성에 따른 맞춤형 전략 선택이 업계 합의
- 시각적 테스트(Visual Testing)가 UI 앱에서 부상

### 6. Observability-Driven Testing
- 로깅, 메트릭, 트레이싱 3가지 관측성 레이어가 프로덕션 테스팅의 전제 조건
- 배포 빈도와 장애율의 상관관계: 관측성 성숙 조직에서는 음의 상관, 미성숙 조직에서는 양의 상관 (IEEE 연구)

---

## 기존 가이드와의 차이점 (What Our Current Guide is Missing)

현재 `/docs/testing/README.md`와 업계 리서치를 비교한 결과, 다음 영역이 보강 또는 추가 필요:

### 보강 필요 항목

| 항목 | 현재 상태 | 업계 수준 |
|------|----------|----------|
| **Test Double 가이드** | Mock 호출 횟수/인자 검증 안티패턴만 언급 | Google의 Real > Fake > Mock 우선순위, Fowler의 Classical vs Mockist 분석, 구체적 사용 지침 필요 |
| **Flaky Test 전략** | "즉시 격리 및 수정" 한 줄 | Google/Spotify의 구체적 전략 (자동 격리, 3연속 실패 규칙, 대시보드 가시성, Flakybot 패턴) |
| **테스트 전략 모델** | Pyramid/Trophy/Diamond 표로 간략 언급 | Honeycomb, Crab 추가 + 아키텍처별 선택 기준 매트릭스 |
| **커버리지 기준 근거** | 80-90% / 70-80% 수치만 제시 | Kent C. Dodds의 "70% 이상에서 수익 체감" 근거, 오픈소스 vs 앱 차이 |

### 신규 추가 필요 항목

| 항목 | 설명 |
|------|------|
| **Contract Testing 섹션** | 마이크로서비스 아키텍처에서 Pact 기반 CDC 가이드. Google, Microsoft, Fowler 모두 권장 |
| **AI 생성 코드 테스팅** | Logic Drift, Dependency Mismatch, Regression Roulette 위험. 비즈니스 불변식 테스트, AI 코드 라벨링, 분기별 감사 |
| **Testing in Production** | Canary 배포, Synthetic Monitoring, Chaos Engineering. Netflix/Google 사례 |
| **Property-Based Testing** | Hypothesis/fast-check 도구, 언제 사용할지 가이드. 일반 Unit Test 대비 50배 효과적 |
| **Test Selection/Prioritization** | Shopify의 Dynamic Analysis, Test Budget 개념. 대규모 프로젝트에서 CI 최적화 |
| **Observability & Testing 연계** | Shift-Left + Shift-Right 통합 Continuous Testing 전략 |
| **DAMP vs DRY 원칙** | Google의 "테스트 코드에서는 DAMP > DRY" 원칙. 현재 가이드에 명시 없음 |
| **테스트 크기(Size) 분류** | Google의 Small/Medium/Large 테스트 크기 프레임워크. 실행 제약 기반 분류 |
| **Record/Replay 패턴** | Google의 외부 서비스 의존성 관리 기법. 대규모 테스트에서 유용 |
| **Subcutaneous Testing** | Fowler의 UI 바로 아래 레이어 테스트 패턴. GUI 취약성 회피 |

---

## Sources

### Martin Fowler / ThoughtWorks
- [The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)
- [Software Testing Guide](https://martinfowler.com/testing/)
- [Mocks Aren't Stubs](https://martinfowler.com/articles/mocksArentStubs.html)
- [Test Pyramid](https://martinfowler.com/bliki/TestPyramid.html)
- [ThoughtWorks Technology Radar](https://www.thoughtworks.com/radar)

### Google
- [Software Engineering at Google - Unit Testing (Ch.12)](https://abseil.io/resources/swe-book/html/ch12.html)
- [Software Engineering at Google - Test Doubles (Ch.13)](https://abseil.io/resources/swe-book/html/ch13.html)
- [Software Engineering at Google - Larger Testing (Ch.14)](https://abseil.io/resources/swe-book/html/ch14.html)
- [Flaky Tests at Google](https://testing.googleblog.com/2016/05/flaky-tests-at-google-and-how-we.html)
- [Google Testing Blog](https://testing.googleblog.com/)

### Kent C. Dodds
- [Write Tests. Not Too Many. Mostly Integration.](https://kentcdodds.com/blog/write-tests)
- [The Testing Trophy and Testing Classifications](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications)
- [Static vs Unit vs Integration vs E2E Testing](https://kentcdodds.com/blog/static-vs-unit-vs-integration-vs-e2e-tests)

### Microsoft
- [Unit Testing Best Practices (.NET)](https://learn.microsoft.com/en-us/dotnet/core/testing/unit-testing-best-practices)
- [Engineering Fundamentals - Unit Testing](https://microsoft.github.io/code-with-engineering-playbook/automated-testing/unit-testing/)
- [PACT Contract Testing](https://devblogs.microsoft.com/ise/pact-contract-testing-because-not-everything-needs-full-integration-tests/)
- [Performance Testing](https://devblogs.microsoft.com/ise/performance-testing/)

### Spotify
- [Test Flakiness - Methods for Identifying and Dealing with Flaky Tests](https://engineering.atspotify.com/2019/11/test-flakiness-methods-for-identifying-and-dealing-with-flaky-tests)

### Shopify
- [Spark Joy by Running Fewer Tests](https://shopify.engineering/spark-joy-by-running-fewer-tests)
- [Test Budget: Time Constrained CI Feedback](https://shopify.engineering/test-budget-time-constrained-ci-feedback)

### Netflix
- [Chaos Engineering](https://newsletter.systemdesign.one/p/chaos-engineering)
- [Testing in Production the Netflix Way](https://launchdarkly.com/blog/testing-in-production-the-netflix-way/)

### Contract Testing
- [Pact Documentation](https://docs.pact.io/)

### Testing Strategy Models
- [web.dev - Pyramid or Crab? Find a testing strategy that fits](https://web.dev/articles/ta-strategies)

### AI & Testing
- [Hidden Risks of AI-Generated Code](https://testkube.io/blog/testing-ai-generated-code)
- [Mutation Testing: Ultimate Guide 2025](https://mastersoftwaretesting.com/testing-fundamentals/types-of-testing/mutation-testing)
- [Property-Based Testing Empirical Evaluation (OOPSLA 2025)](https://dl.acm.org/doi/10.1145/3764068)
