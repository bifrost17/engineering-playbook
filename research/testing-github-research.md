# GitHub 테스팅 베스트 프랙티스 리서치 결과

> 리서치 일자: 2026-04-08
> 주요 소스: GitHub 고스타 리포지토리, Google/Microsoft/Thoughtworks 엔지니어링 가이드, 업계 저명 저자 블로그

---

## GitHub 리서치 결과

### 1. goldbergyoni/javascript-testing-best-practices (24,598 stars)

- **URL**: https://github.com/goldbergyoni/javascript-testing-best-practices
- **신뢰도**: 매우 높음 - JavaScript/Node.js 테스팅 분야 최고 인기 리포지토리

#### 핵심 원칙

**골든 룰: 테스트는 단순해야 한다**
- 테스트 코드는 프로덕션 코드가 아니다 - 짧고, 단순하고, 평평하고, 즐거운 코드여야 한다
- "System 1" 사고(직관적/자동적 사고)를 활용할 수 있도록 설계해야 한다
- 이미 프로덕션 코드로 머리가 복잡한 상태에서 추가적인 복잡성을 감당할 여유가 없다

**테스트 구조 (AAA 패턴)**
1. **Arrange** (준비): 설정 코드, 유닛 인스턴스화, 모의 객체, 데이터 준비
2. **Act** (실행): 테스트 대상 유닛 실행 (보통 한 줄)
3. **Assert** (검증): 기대값 확인 (보통 한 줄)

**테스트 이름 작성법 - 세 가지 요소 포함**
1. 무엇을 테스트하는가 (테스트 대상 유닛)
2. 어떤 상황에서 (시나리오/조건)
3. 기대되는 결과는 무엇인가

예시: `"가격이 지정되지 않은 경우, 상품 상태는 승인 대기 중이어야 한다"`

**BDD 스타일의 선언적 어설션**
- 조건문과 반복문이 포함된 명령형 코드 대신, 사람이 읽을 수 있는 선언적 어설션 사용
- `.to.include.ordered.members()` 같은 유창한(fluent) 어설션으로 자기 문서화

**블랙박스 테스트 고수**
- 퍼블릭 메서드와 동작만 테스트, 내부 구현 세부사항 테스트 금지
- 실제 문제가 있을 때만 테스트가 깨져야 한다
- 내부 리팩토링이 테스트를 깨뜨리면 안 된다

#### 주요 섹션 구성
- Section 0: 골든 룰
- Section 1: 테스트 해부학 (12가지 실천법)
- Section 2: 백엔드 테스팅 (13가지 실천법)
- Section 3: 프론트엔드 테스팅 (11가지 실천법)
- Section 4: 테스트 효과 측정 (4가지 실천법)
- Section 5: CI (9가지 실천법)

#### 고유 인사이트
- 테스트 코드의 신뢰성보다 민첩성(agility)을 우선시하는 전략적 트레이드오프 허용
- 테스트를 라이브 앱 문서로 활용하여 개발자와 고객 간 커뮤니케이션 도구 역할

---

### 2. goldbergyoni/nodejs-testing-best-practices (4,335 stars)

- **URL**: https://github.com/goldbergyoni/nodejs-testing-best-practices
- **신뢰도**: 높음 - Node.js 전문 테스팅 가이드, 실용적 예제 앱 포함

#### 핵심 원칙

**컴포넌트/통합 테스트부터 시작**
- "Testing Diamond" 모델 채택: 컴포넌트 수준에서 시작하여 API를 통해 전체 서비스 테스트
- 실제 데이터베이스 사용, 외부 의존성만 가짜(fake)로 대체
- 컴포넌트 테스트가 버그의 약 99%를 포착

**E2E 테스트는 최소한으로**
- 3~10개의 E2E 테스트만 포함
- 컴포넌트 테스트로 감지할 수 없는 설정 문제, 인프라 문제, 서드파티 통합에 집중
- 유닛 테스트는 복잡한 알고리즘이나 비자명한(non-trivial) 로직에만 사용

**백엔드의 5가지 출구(Exit Door) 테스트**
1. **응답(Response)**: 응답 데이터 정확성, 스키마 구조, HTTP 상태 코드
2. **상태 변경(State Changes)**: 데이터베이스에 올바르게 저장되었는지 확인
3. **외부 서비스 호출**: HTTP 호출, SMS/이메일 발송, 결제 등
4. **메시지 큐 발행**: MQ 시스템에 메시지가 올바르게 전달되었는지
5. **관찰 가능성(Observability)**: 올바른 에러 로깅, 메트릭 발생, 모니터링 데이터

#### 인프라 설정 베스트 프랙티스

**Docker-Compose 활용**
- 모든 데이터베이스, 메시지 큐, 지원 서비스를 Docker-Compose로 실행
- `git clone && npm test` 한 번으로 모든 것이 동작해야 함

**실제 데이터베이스 최적화**
- PostgreSQL: `fsync=off`, `synchronous_commit=off`, `full_page_writes=off` 설정
- 20~40% 성능 향상 달성
- 인메모리 엔진(SQLite)은 멀티 프로세스 테스트에서 성능 저하
- 데이터베이스 모킹은 리스크 커버리지를 크게 감소

**웹 서버를 테스트와 동일 프로세스에서 실행**
- 테스트 더블을 사용한 예외 상황 시뮬레이션 가능
- 환경 변수 중간 변경 가능
- 코드 커버리지 정확한 측정 가능

#### 성능 목표
- 40개 컴포넌트 테스트를 5초 이내에 완료 (데이터베이스 작업 포함)
- 인프라 유지 시 테스트 시작 시간: 3~5ms
- 글로벌 셋업은 1회만 수행

#### 고유 인사이트
- "기능(Feature)을 테스트하라, 함수(Function)를 테스트하지 마라"
- 개발 중에 테스트 작성 - 구현 후가 아닌 구현과 동시에
- 로컬에서는 컨테이너를 계속 유지하고, CI에서만 정리

---

### 3. Google Software Engineering at Google (abseil.io/swe-book)

- **URL**: https://abseil.io/resources/swe-book/html/ch11.html, ch12.html, ch14.html
- **신뢰도**: 최상 - Google 내부 10년+ 경험 축적

#### 테스트 크기 분류 (Small/Medium/Large)

| 구분 | Small | Medium | Large |
|------|-------|--------|-------|
| 프로세스 | 단일 프로세스 | 다중 프로세스 | 제한 없음 |
| I/O | 금지 | localhost만 허용 | 제한 없음 |
| 네트워크 | 금지 | localhost만 | 원격 허용 |
| 속도 | 밀리초 | 초 단위 | 분 단위 |
| 결정성 | 매우 높음 | 중간 | 낮음 |

#### 테스트 피라미드 비율
- **80% 유닛 테스트** (좁은 범위)
- **15% 통합 테스트** (중간 범위)
- **5% E2E 테스트** (넓은 범위)

#### 유닛 테스트 핵심 원칙

**변하지 않는 테스트(Unchanging Tests)**
- 테스트는 최초 작성 후 안정적으로 유지되어야 함
- 4가지 프로덕션 변경 유형별 테스트 영향:
  1. 순수 리팩토링 → 테스트 변경 불필요
  2. 새로운 기능 → 기존 테스트 유지
  3. 버그 수정 → 누락된 케이스에 대한 새 테스트 추가
  4. 동작 변경 → 유일하게 테스트 수정이 필요한 경우

**퍼블릭 API를 통한 테스트**
- "시스템의 사용자와 동일한 방식으로 호출하라"
- 내부 구현 테스트는 리팩토링 시 깨지는 취약한 테스트를 만든다

**상태를 테스트하라, 상호작용을 테스트하지 마라**
- 시스템의 결과 상태를 검증하라
- 어떻게 도달했는지(상호작용)가 아닌 무엇이 되었는지(상태)를 확인

**DAMP > DRY**
- "Descriptive And Meaningful Phrases" (서술적이고 의미 있는 표현)
- 테스트 코드에서는 DRY보다 명확성과 자기 완결성을 우선
- 일부 중복은 테스트를 더 단순하고 이해하기 쉽게 만든다면 허용

**테스트에 로직을 넣지 마라**
- "테스트를 검증하기 위해 테스트가 필요하다면, 뭔가 잘못된 것이다"
- 직선적인(straight-line) 코드만 사용, 복잡한 조건문이나 반복문 금지

#### 대규모 테스트 관련 원칙

**충실도(Fidelity) vs 격리(Isolation) 트레이드오프**
- 격리된 SUT: 플래키니스 감소, 프로덕션 충실도 저하
- 프로덕션 유사 SUT: 충실도 증가, 인프라 플래키니스 증가

**유닛 테스트로 커버할 수 없는 영역**
- 불충실한 테스트 더블 (실제 구현과 다른 모의 객체)
- 설정 문제 ("설정 변경이 주요 장애의 가장 큰 원인")
- 부하/성능 시나리오
- 예상치 못한 동작 (Hyrum's Law)

**Beyonce Rule**: "좋다면, 테스트를 붙였어야 한다" - 성능, 정확성, 접근성, 보안, 장애 처리 모두 포함

#### 플래키니스 관리
- Google 목표: 약 0.15% 플래키 비율
- 0.1% 실패율에서 하루 10,000개 테스트 실행 시 → 매일 10개의 거짓 실패 조사
- 1%에 가까워지면 엔지니어가 테스트 시스템 자체를 불신

#### 고유 인사이트
- 테스트 크기(Size)와 범위(Scope)는 독립적인 차원
- "Ice cream cone" 안티패턴: 프로토타입에 수동 테스트가 쌓여 며칠 만에 레거시 코드 생성
- A/B Diff 회귀 테스트: Google에서 가장 흔한 대규모 테스트 유형
- Chaos Engineering과 DiRT(재해 복구 훈련) 활용
- 테스트 소유권 미지정 시: 유지보수 부재, 느린 실패 해결, 기여자 업데이트 어려움

---

### 4. Microsoft Engineering Fundamentals Playbook (2,635 stars)

- **URL**: https://github.com/microsoft/code-with-engineering-playbook
- **문서**: https://microsoft.github.io/code-with-engineering-playbook/automated-testing/
- **신뢰도**: 높음 - Microsoft ISE(Industry Solutions Engineering) 팀의 실전 경험

#### 핵심 원칙

**필수 요구사항**
- "테스트가 동반되지 않은 코드는 불완전한 것으로 간주"
- PR 머지 전 유닛 테스트 반드시 실행
- 테스트 실패 시 코드 머지 차단
- 통합/E2E 테스트는 전체 시스템에서 정기적으로 실행

**테스트 유형 포괄적 커버리지**
1. 유닛 테스팅: 예상, 엣지, 예외 입력에 대한 로직 검증
2. 통합 테스팅: 여러 인터페이스의 컴포넌트 상호작용
3. E2E 테스팅: 다수 컴포넌트의 함께 작동 검증
4. CDC(Consumer-Driven Contract) 테스팅: Pact 사용
5. 성능 테스팅: 부하, 스트레스, 스파이크, 소크 테스트
6. 합성 모니터링: 가용성 저하 및 회귀 감지
7. 섀도우 테스팅: 하위 호환성 입증
8. UI 테스팅: 접근성 및 사용성 자동 검증
9. 결함 주입 테스팅: 장애 상황에서의 복원력 검증

**유닛 테스트 3대 속성**
- **입증 가능한 신뢰성**: 100% 신뢰할 수 있어 실패 시 코드 버그를 의미
- **빠른 속도**: 밀리초 단위 실행, 전체 스위트 수 초 이내
- **격리**: 모든 외부 의존성 제거

**안티패턴**
- Sleep 사용: 신뢰할 수 없는 의존성 지표, 설계 결함 시사
- 디스크 I/O: 시스템 드라이브 의존성 생성
- 서드파티 API 직접 호출: 인터페이스로 감싸서 사용

#### 테스트 가능한 애플리케이션 구축 요건
- 매개변수화된 설정 (하드코딩 금지, 합리적 기본값 사용)
- 시작 시 모든 매개변수 로깅
- 콘솔과 외부 시스템 이중 로깅
- 시작/완료 메시지가 포함된 활동 로깅
- 분산 추적을 위한 Correlation ID
- 컨텍스트 메타데이터 (테넌트 ID, 고객 ID, 주문 ID)
- 함수 수준의 성능 메트릭 로깅

#### 테스트 케이스 정의 형식
- TestCase Title
- Preconditions
- Test Steps
- Expected Result
- Given-When-Then 형식 사용

#### 고유 인사이트
- 결과(Outcomes) → 기법(Techniques) 매핑 접근법
- 하위 호환성, 텔레메트리 완전성, 논리적 정확성, 회귀 방지, 재해 복구, 보안 취약점, 용량 계획 등 결과별 테스트 기법 지정

---

### 5. NoriSte/ui-testing-best-practices (1,732 stars)

- **URL**: https://github.com/NoriSte/ui-testing-best-practices
- **신뢰도**: 높음 - UI 테스팅 분야 최대 베스트 프랙티스 목록

#### 핵심 원칙

**테스트 유형 효과성 순서**
1. UI 통합 테스트가 가장 효과적
2. E2E 테스트가 가장 높은 신뢰도 제공
3. 컴포넌트 테스트가 가장 빠른 격리 테스트 가능

**실전 지침**
- **Await, Don't Sleep**: "Sleep은 테스트를 느리고 취약하게 만든다" - 고정 지연 대신 결정적 이벤트 대기
- **UI 없이 상태 설정**: API, 데이터베이스, 직접 상태 주입 사용
- **추상화 vs 디버그 가능성**: "추상화가 높을수록 디버깅이 어렵다" - 최소한의 헬퍼 레이어
- **크로스 브라우저 테스트는 과대평가**: 기능 테스트와 시각적 테스트를 분리
- **버그 주도 테스트**: 버그 발견 시 먼저 재현 테스트 작성, 그 다음 수정

**서버 통신 테스트**
- "모든 XHR 요청을 신중하게 검사해야 한다"
- 백엔드 스키마를 export하여 프론트엔드와 검증
- 백엔드 변경이 프론트엔드를 모르게 깨뜨리는 것을 방지

**Siemens 모델**: 프론트엔드는 통합 테스트로, 백엔드는 E2E 테스트로

#### 안티패턴
- 고정 지연(sleep) 사용
- 과도한 테스트 헬퍼 추상화
- 요청/응답 검증 누락
- 백엔드 스키마 미인식 상태의 테스트
- 플래키 테스트를 정상으로 수용
- 복잡한 테스트 의존성 체인
- 근거 없는 과도한 크로스 브라우저 테스트

#### 고유 인사이트
- 시각적 회귀 테스트는 "어렵다" - Applitools, Percy 같은 프리미엄 서비스 사용 권장
- 매개변수 조합 테스팅: "대부분의 소프트웨어 버그는 1~2개의 매개변수로 발생"
- 초보자는 "Top-to-Bottom" 접근: 유닛 테스트가 아닌 통합/E2E부터 시작하여 첫날부터 높은 신뢰도 확보

---

### 6. Google Engineering Practices (20,506 stars)

- **URL**: https://google.github.io/eng-practices/
- **리포지토리**: https://github.com/google/eng-practices
- **신뢰도**: 최상 - Google 조직 전체 실천 경험

#### 핵심 원칙
- 코드 리뷰 시 테스트 동반 필수
- CL(Changelist)은 자기 완결적 변경 단위
- 리뷰어와 저자 양쪽 모두를 위한 가이드라인 분리

#### 고유 인사이트
- Google Testing Blog: "Tech on the Toilet (TotT)" - 매주 1페이지 소프트웨어 개발 출판물
- Google 사무실 화장실에 게시, 외부 공유 가치가 있는 에피소드는 블로그에 게시
- 최근 주요 주제 (2025~2026):
  - TDD의 Red-Green-Refactor 사이클 (2026.03)
  - 플래그의 안전한 기본값 설정 (2026.03)
  - "Functional Core, Imperative Shell" 패턴 (2025.10) - 순수 비즈니스 로직과 사이드 이펙트 분리

---

### 7. Martin Fowler - The Practical Test Pyramid

- **URL**: https://martinfowler.com/articles/practical-test-pyramid.html
- **신뢰도**: 최상 - 소프트웨어 엔지니어링 분야 세계적 권위자

#### 핵심 원칙

**테스트 피라미드 3계층**
- 많은 작고 빠른 유닛 테스트 작성
- 약간의 조금 더 거친 테스트 작성
- 매우 적은 수의 고수준 E2E 테스트 작성

**두 가지 핵심 교훈**
1. 다양한 세분성의 테스트를 작성하라
2. 높은 수준으로 갈수록 테스트 수를 줄여라

**안티패턴: 테스트 아이스크림 콘**
- 과도한 고수준 테스트: 느리고 유지보수 어려움

**통합 테스트: "좁은" 접근법**
- 한 번에 하나의 통합 지점만 테스트
- 외부 서비스는 테스트 더블로 대체하면서 통합 로직 테스트

**CDC(Consumer-Driven Contract) 테스팅**
- 소비자가 계약 테스트 작성 → 공급자가 지속적으로 실행
- Pact 프레임워크로 구현

**파이프라인 통합**
- 빠른 피드백이 배치 결정: 유닛 테스트(초) → 통합 테스트(분) → E2E 테스트(더 긴 시간)

**테스트 중복 회피 규칙**
1. 상위 테스트에서 에러를 잡는데 하위 테스트가 없다면, 하위 테스트를 작성하라
2. 테스트를 피라미드 아래로 최대한 밀어라

#### "다양한 테스트 모양" (2021 후속 글)

- 업계에서 "유닛 테스트"와 "통합 테스트"의 정의가 일관되지 않음
- 피라미드, 트로피, 다이아몬드, 벌집 등 다양한 모델 존재
- **진짜 중요한 것**: "어떤 비율의 테스트를 작성할지 논쟁하는 것은 방해가 된다. 명확한 경계를 설정하고, 빠르고 안정적으로 실행되며, 유용한 이유로만 실패하는 표현력 있는 테스트를 작성하는 팀은 거의 없다"

#### 고유 인사이트
- Solitary tests vs Sociable tests: 모의 객체 사용 여부에 따른 유닛 테스트 구분
- 사적(private) 메서드 테스트 필요성은 설계 문제의 지표
- DAMP(Descriptive And Meaningful Phrases) > DRY in test code
- Rule of Three: 테스트 유틸리티 리팩토링 전 3번 반복을 기다려라

---

### 8. Kent C. Dodds - Testing Trophy & Testing Library

- **URL**: https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications
- **URL**: https://kentcdodds.com/blog/write-tests
- **신뢰도**: 높음 - React Testing Library 창시자, 프론트엔드 테스팅 사실상 표준

#### Testing Trophy 모델

전통적 피라미드 대신 4계층 트로피 구조:
1. **Static** (기반): 타입 체크, 린팅 (TypeScript, ESLint)
2. **Unit** (보조): 개별 함수/클래스/객체, 의존성 없거나 모킹
3. **Integration** (최대 비중): 여러 유닛의 협동, HTTP 요청만 모킹
4. **E2E** (최상단): 최소 모킹, 실제 사용자 시나리오

#### 핵심 철학: "Write tests. Not too many. Mostly integration."

**Write tests**
- 테스트는 초기 투자에도 불구하고 전체적으로 시간을 절약
- 정적 타이핑으로도 비즈니스 로직 정확성은 검증 불가

**Not too many**
- "커버리지가 70%를 훨씬 넘으면 수확 체감이 시작된다"
- 100% 커버리지 추구는 역효과 - 로직 없는 사소한 코드를 테스트하게 됨
- 구현 세부사항 테스트는 거짓 자신감과 미래 변경 복잡성 초래
- "코드를 리팩토링할 때 테스트를 변경해야 하는 일은 거의 없어야 한다"

**Mostly integration**
- "피라미드 위로 올라갈수록 각 테스트 형태의 신뢰도 지수가 증가"
- 통합 테스트가 속도와 신뢰도의 최적 균형

**과도한 모킹 금지**
- "무언가를 모킹하면, 테스트 대상과 모킹된 것 사이의 통합에 대한 모든 신뢰를 제거하는 것"
- React에서 shallow rendering 회피

#### 핵심 원칙
> "테스트가 소프트웨어의 실제 사용 방식과 비슷할수록, 더 많은 신뢰를 줄 수 있다"

- 구현 세부사항 테스트 금지 - 사용자가 보지 않는 상태 변수, 이벤트 핸들러 등
- 코드가 아닌, 코드가 최종 사용자와 개발자에게 미치는 관찰 가능한 효과를 테스트

#### 고유 인사이트
- "유닛 테스트"의 정의가 24개 이상 존재 - 비율 논쟁은 비생산적
- Testing Trophy는 개별 코드베이스와 모놀리식 아키텍처에 적용, 마이크로서비스에는 반드시 적용되지 않음
- React Testing Library가 React 앱의 사실상 표준 테스팅 라이브러리

---

### 9. Thoughtworks - 테스팅 7대 원칙

- **URL**: https://www.thoughtworks.com/en-us/insights/blog/testing/seven-guiding-principles-testing
- **신뢰도**: 높음 - 글로벌 소프트웨어 컨설팅 리더

#### 7대 원칙

1. **최종 사용자가 유일한 친구**: 비즈니스 요구사항이나 기술적 세부사항이 아닌, 최종 사용자 관점에서 테스트
2. **미시적/거시적 수준의 테스팅**: 작은 기능의 엣지 케이스(미시)와 기능 흐름/모듈 간 데이터 전파(거시) 사이를 지속적으로 줌인/줌아웃
3. **빠른 피드백**: "결함은 배포 주기 후반에 발견될수록 비용이 증가" - shift-left 테스팅, dev-box 테스팅, CI 자동화
4. **지속적 피드백**: 모든 자동화 테스트를 CI에 통합, 모든 커밋마다 실행, 필요시 병렬화
5. **품질 정량화**: 자동화 테스트 결함 포착률, 커밋-배포 시간, 자동화 배포 수, 회귀 결함, 프로덕션 결함, 사용성 점수
6. **커뮤니케이션과 협업**: 스탠드업, 스토리 킥오프, 플래닝 미팅, ADR, 테스트 전략 문서를 통한 교차기능 지식 공유
7. **결함 탐지보다 결함 예방**: Three Amigos, IPM, 스토리 킥오프, ADR, shift-left 테스팅, TDD, 페어 프로그래밍, 쇼케이스

#### 자동화 테스트 구조화 가이드라인

**4대 가이드라인**
1. **구조**: 유닛 테스트 최우선, 테스트 피라미드 모델 준수
2. **속도**: 유닛(5~10분), 통합(5~15분), 수락(10~30분), 성능(하루 여러 번 장시간)
3. **견고성**: 시간 의존 테스트, 랜덤 실패 브라우저 테스트, 외부 시스템 의존 테스트 회피
4. **디버깅**: "테스트 실패 시 원인 파악에 오래 걸리면, 테스트에 문제가 있다"

#### 고유 인사이트
- "진화적 테스트 전략": 솔루션/비즈니스 변경과 함께 테스트 전략도 진화
- 테스터를 솔루션 설계와 비즈니스 토론에 포함
- Four Key Metrics로 코드 안정성과 배포 속도 측정

---

### 10. 18F Automated Testing Playbook (미국 정부)

- **URL**: https://github.com/18F/automated-testing-playbook
- **신뢰도**: 중간 - 미국 연방 정부 디지털 서비스 팀, Google Test Certified 프로그램 기반

#### 핵심 원칙
- Google의 Test Certified 프로그램에 느슨하게 기반
- 자동화 테스트 관행 개선을 위한 로드맵 제공
- 공공 부문 맞춤 원칙, 실천법, 전략

---

### 11. 2025~2026 테스트 자동화 트렌드

- **소스**: BrowserStack, Katalon, Parasoft, bugbug.io, TestDevLab 등 다수
- **신뢰도**: 중간~높음 - 업계 실무 트렌드

#### 주요 동향
- 81%의 개발팀이 AI를 테스팅 워크플로우에 사용 (2026)
- 72%의 QA 전문가가 테스트 생성 및 스크립트 최적화에 AI 활용
- "하이브리드"가 사실상 기본: 단일 접근법으로는 모든 계층 커버 불가
- 자동화해야 할 테스트: 명확한 pass/fail 결과, 시간 소모적, 높은 실패 리스크, 안정적 기능

#### 핵심 실천법
- CI/CD 파이프라인 통합이 핵심 - 모든 코드 변경 시 자동 실행
- 자동화 코딩 표준 준수: 명명 규칙, 구조, 에러 처리의 일관성
- 자동화는 "안전망이 아닌 강력한 도우미" - 거버넌스, 모니터링, 인간 판단 필요
- AI 기반 자가 치유(self-healing) 프레임워크 부상

---

## 전체 소스 공통 테마 종합

### 1. 테스트 구조와 가독성

| 공통 원칙 | 동의하는 소스 |
|-----------|--------------|
| AAA(Arrange-Act-Assert) 패턴 | Google, Microsoft, Goldbergyoni, Fowler |
| Given-When-Then 형식 | Microsoft, Thoughtworks, Google |
| 하나의 테스트에 하나의 동작 검증 | 모든 소스 |
| DAMP > DRY (테스트 코드에서) | Google, Fowler, Goldbergyoni |
| 테스트에 로직 넣지 않기 | Google, Goldbergyoni |
| 명확한 실패 메시지 | Google, Microsoft |

### 2. 테스트 범위와 전략

| 모델 | 제안자 | 핵심 차이 |
|------|--------|-----------|
| 테스트 피라미드 (80/15/5) | Google, Microsoft, Fowler, Thoughtworks | 유닛 테스트 최다 |
| 테스팅 트로피 | Kent C. Dodds | 통합 테스트 최대 비중 |
| 테스팅 다이아몬드 | Goldbergyoni (Node.js) | 컴포넌트 테스트 중심 |
| 맥락에 따라 선택 | Fowler (2021 후속글) | "비율 논쟁은 방해" |

### 3. 블랙박스 테스트 원칙 (전체 합의)

- **공개 API만 테스트**: Google, Goldbergyoni, Dodds, Fowler 모두 동의
- **구현 세부사항 테스트 금지**: 리팩토링을 안전하게 만드는 핵심
- **동작(Behavior) 테스트**: 메서드가 아닌, 사용자에게 관찰 가능한 동작 검증
- **상태 > 상호작용**: 결과를 검증하지, 과정을 검증하지 마라

### 4. 커버리지 전략 (합의와 차이)

| 소스 | 커버리지 가이드라인 |
|------|-------------------|
| Google | 80% 유닛 / 15% 통합 / 5% E2E. 커버리지 %보다 고객 기대 충족 여부 |
| Dodds | 70% 이후 수확 체감. 100% 추구 역효과 |
| Fowler | 사소한 코드 테스트로 100% 달성은 시간 낭비 |
| Goldbergyoni | 커버리지 보고서로 핵심 기능/경로 커버 확인 |
| Microsoft | 코드는 테스트 없이 불완전 |

### 5. 모킹과 테스트 더블 (미묘한 차이)

| 접근법 | 소스 | 요점 |
|--------|------|------|
| 최소 모킹 | Dodds, Goldbergyoni (Node.js) | 과도한 모킹 = 통합 신뢰 상실 |
| 실제 DB 사용 | Goldbergyoni (Node.js) | 인메모리 DB보다 실제 DB + 최적화 |
| 외부만 모킹 | Goldbergyoni (JS), Fowler | 외부 서비스만 테스트 더블 |
| 불충실한 더블 주의 | Google (Ch.14) | 모의 객체가 실제 구현과 다를 위험 |
| 격리 vs 사교적 테스트 | Fowler | 두 접근법 모두 유효 |

### 6. 플래키니스(불안정) 관리 (전체 합의)

- Sleep/고정 지연 사용 금지 → 이벤트/폴링 기반 대기 (Google, NoriSte, Microsoft)
- 결정적(deterministic) 테스트 작성 (모든 소스)
- 플래키 테스트는 테스트 시스템 신뢰를 파괴 (Google: 0.15% 목표)
- 타임아웃을 테스트 환경에 맞게 조정 가능하게 (Google)

### 7. 테스트 작성 시기

| 소스 | 권장 시기 |
|------|-----------|
| Goldbergyoni (Node.js) | 기능의 기대 동작을 이해한 직후, 내부 함수 구현 전 |
| Google | 코드 리뷰 시 테스트 동반 필수 |
| Thoughtworks | Shift-left, TDD, 스토리 킥오프 시 테스트 논의 |
| Google Blog (2026) | TDD Red-Green-Refactor 사이클 |

### 8. CI/CD 통합 (전체 합의)

- 모든 소스가 CI/CD 파이프라인에 테스트 자동 통합 강조
- 빠른 테스트를 먼저, 느린 테스트를 나중에 실행 (파이프라인 단계별 배치)
- 테스트 실패 시 머지/배포 차단
- 병렬 실행으로 피드백 루프 단축

### 9. 프론트엔드 특화 공통 테마

- 사용자와 동일한 방식으로 테스트 (Dodds의 핵심 원칙)
- UI 없이 상태 설정 (NoriSte)
- 시각적 회귀 테스트는 전문 서비스 사용 (NoriSte)
- 크로스 브라우저 테스트는 과대평가 (NoriSte)
- React Testing Library가 사실상 표준 (Dodds)

### 10. 고유하고 덜 알려진 인사이트 모음

1. **Google "Beyonce Rule"**: 좋다면 테스트를 붙여라 - 모든 시스템 속성에 적용
2. **Goldbergyoni "5가지 출구"**: 백엔드 테스트의 5가지 관찰 가능한 결과
3. **Google A/B Diff 테스트**: 구버전과 신버전에 동일 트래픽 전송 후 차이 비교
4. **NoriSte "매개변수 조합"**: 대부분의 버그가 1~2개 매개변수 조합에서 발생
5. **Fowler "Rule of Three"**: 유틸리티 리팩토링 전 3번 반복 대기
6. **Google "Functional Core, Imperative Shell"**: 순수 로직과 사이드 이펙트 분리로 테스트 용이성 극대화
7. **Goldbergyoni "RAM에 테스트 데이터 저장"**: tmpfs 활용으로 성능 최적화
8. **Google "테스트 소유권"**: OWNERS 파일 또는 테스트별 어노테이션으로 소유권 문서화
9. **NoriSte "Siemens 모델"**: 프론트엔드는 통합, 백엔드는 E2E로 분리
10. **Thoughtworks "진화적 전략"**: 테스트 전략은 비즈니스/솔루션 변경과 함께 진화

---

## 참고 소스 목록

### 고스타 GitHub 리포지토리
- [goldbergyoni/javascript-testing-best-practices](https://github.com/goldbergyoni/javascript-testing-best-practices) (24,598 stars)
- [google/eng-practices](https://github.com/google/eng-practices) (20,506 stars)
- [goldbergyoni/nodejs-testing-best-practices](https://github.com/goldbergyoni/nodejs-testing-best-practices) (4,335 stars)
- [microsoft/code-with-engineering-playbook](https://github.com/microsoft/code-with-engineering-playbook) (2,635 stars)
- [TheJambo/awesome-testing](https://github.com/TheJambo/awesome-testing) (2,234 stars)
- [NoriSte/ui-testing-best-practices](https://github.com/NoriSte/ui-testing-best-practices) (1,732 stars)
- [18F/automated-testing-playbook](https://github.com/18F/automated-testing-playbook) (55 stars)

### 서적 및 공식 문서
- [Software Engineering at Google - Ch.11: Testing Overview](https://abseil.io/resources/swe-book/html/ch11.html)
- [Software Engineering at Google - Ch.12: Unit Testing](https://abseil.io/resources/swe-book/html/ch12.html)
- [Software Engineering at Google - Ch.14: Larger Testing](https://abseil.io/resources/swe-book/html/ch14.html)
- [Microsoft Engineering Fundamentals Playbook - Testing](https://microsoft.github.io/code-with-engineering-playbook/automated-testing/)

### 블로그 및 아티클
- [Martin Fowler - The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)
- [Martin Fowler - On the Diverse And Fantastical Shapes of Testing](https://martinfowler.com/articles/2021-test-shapes.html)
- [Kent C. Dodds - The Testing Trophy and Testing Classifications](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications)
- [Kent C. Dodds - Write tests. Not too many. Mostly integration.](https://kentcdodds.com/blog/write-tests)
- [Google Testing Blog](https://testing.googleblog.com/)
- [Thoughtworks - Seven Guiding Principles in Testing](https://www.thoughtworks.com/en-us/insights/blog/testing/seven-guiding-principles-testing)
- [Thoughtworks - Guidelines for Structuring Automated Tests](https://www.thoughtworks.com/insights/blog/guidelines-structuring-automated-tests)
