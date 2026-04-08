# TDD (Test-Driven Development) Research

> 작성일: 2026-04-08
> 목적: Engineering Playbook TDD 가이드 작성을 위한 업계 베스트 프랙티스 리서치

---

## 1. TDD 핵심 개념 및 사이클

### 1.1 Red-Green-Refactor 사이클

**CONSENSUS**

- **Kent Beck의 Canon TDD (2023 재정의)**: 5단계 — (1) 테스트 시나리오 목록 작성 (2) 목록에서 하나를 실행 가능한 테스트로 전환 (3) 코드를 변경하여 해당 테스트 + 이전 모든 테스트 통과 (4) 선택적 리팩토링 (5) 목록이 빌 때까지 반복 ([Canon TDD - Kent Beck](https://tidyfirst.substack.com/p/canon-tdd))
- **흔한 오해**: "모든 테스트를 먼저 작성한 다음 코딩한다"는 TDD가 아님. 한 번에 하나의 테스트만 작성하고 통과시킨다.
- **TDD의 4가지 목표**: (1) 이전 동작 유지 (2) 새 행위가 예상대로 동작 (3) 다음 변경에 대비된 상태 (4) 프로그래머의 확신
- **두 가지 설계 차원**: 인터페이스/논리 설계(행위가 어떻게 호출되는가) vs 구현/물리 설계(시스템이 행위를 어떻게 실행하는가)

**Uncle Bob의 TDD 사이클 계층 구조** ([The Cycles of TDD](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html)):

| 사이클 | 시간 척도 | 내용 |
|--------|-----------|------|
| Nano-cycle (3법칙) | 초 단위 | 한 줄 수준의 test-code 교대 |
| Micro-cycle (Red-Green-Refactor) | 분 단위 | 실패 테스트 → 통과 → 정리 |
| Milli-cycle (Specific/Generic) | ~10분 | 테스트 구체화 → 코드 일반화 |
| Primary cycle (Boundaries) | 시간 단위 | 아키텍처 경계 존중 |

> Sources:
> - [Canon TDD - Kent Beck Substack](https://tidyfirst.substack.com/p/canon-tdd)
> - [The Cycles of TDD - Uncle Bob](https://blog.cleancoder.com/uncle-bob/2014/12/17/TheCyclesOfTDD.html)

---

### 1.2 Uncle Bob의 TDD 3법칙

**CONSENSUS**

1. **실패하는 테스트를 작성하기 전에는 프로덕션 코드를 작성하지 않는다.**
2. **실패하기에 충분한 것 이상의 테스트를 작성하지 않는다** (컴파일 실패도 실패에 포함).
3. **현재 실패하는 테스트를 통과시키기에 충분한 것 이상의 프로덕션 코드를 작성하지 않는다.**

나노 사이클(초 단위)에서 작동하며, 하나의 유닛 테스트 완성 전에 약 12번 반복. Uncle Bob은 1999년 Kent Beck과의 페어 프로그래밍에서 이 리듬을 배웠다고 밝힘.

> Sources:
> - [The Three Rules of TDD - Uncle Bob](http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd)

---

### 1.3 Kent Beck의 Green Bar 패턴

**CONSENSUS**

| 패턴 | 설명 | 사용 시점 |
|------|------|----------|
| **Fake It ('Til You Make It)** | 하드코딩된 상수 반환 → 점진적으로 실제 구현으로 대체 | 구현 방향이 불확실할 때 |
| **Triangulation** | 2개 이상 테스트로 상수 반환 불가능하게 만들어 올바른 추상화 유도 | 올바른 추상화가 불확실할 때 |
| **Obvious Implementation** | 구현이 명확할 때 바로 실제 코드 작성. 예상치 못한 실패 시 Fake It으로 전환 | 구현이 명확하고 자신감이 있을 때 |

**세 패턴의 협업**: Obvious Implementation으로 순조롭게 진행하다가 예상치 못한 실패 → Fake It으로 후퇴 → 리팩토링으로 올바른 코드 도출 → 자신감 회복 시 다시 Obvious Implementation.

> Sources:
> - Kent Beck, *Test-Driven Development: By Example* (2002)
> - [TDD Patterns Gist 정리](https://gist.github.com/jameshwang/5335032)

---

## 2. TDD 변형 및 스타일

### 2.1 Classical (Detroit) vs London (Mockist) TDD

**CONSENSUS**

| 차원 | Classical (Detroit/Chicago) | London (Mockist) |
|------|---------------------------|-------------------|
| "단위" 정의 | 모듈, 클래스 집합 (유연) | 단일 클래스 |
| Test Double 사용 | 꼭 필요할 때만 (DB, 네트워크) | 모든 협력 객체에 mock 사용 |
| 검증 방식 | 상태 검증 (State Verification) | 행위 검증 (Behavior Verification) |
| 개발 방향 | Inside-out (도메인 모델부터) | Outside-in (API/컨트롤러부터) |
| 리팩토링 내성 | 높음 (구현 변경에 테스트 불변) | 낮음 (구현 결합으로 테스트 수정 필요) |

- Martin Fowler 자신은 Classical 접근을 선호하며, 실무에서는 양쪽의 장점을 상황에 맞게 조합하는 것을 권장.

**DECISION NEEDED**

- 팀 기본 스타일을 Classical vs London 중 어느 쪽으로 권장할지 (또는 상황별 가이드)

> Sources:
> - [Mocks Aren't Stubs - Martin Fowler](https://martinfowler.com/articles/mocksArentStubs.html)

---

### 2.2 관련 방법론

**CONSENSUS**

**BDD (Behavior-Driven Development)** — Dan North (2003/2006):
- TDD를 가르치면서 겪은 좌절에서 출발. "Test"를 "Behaviour"로 재명명하여 의미론적 전환
- **Given-When-Then 형식**: 비개발자도 읽을 수 있는 실행 가능한 사양
- TDD를 대체하는 것이 아니라 진화한 것. "TDD를 배울 때 겪는 모든 함정을 피하는 접근법"
- 도구: Cucumber, SpecFlow, JBehave, behave (Python)

**ATDD (Acceptance TDD)**:
- 비즈니스 고객, 개발자, 테스터 간 소통 기반. TDD가 ATDD의 "내부 사이클"로 활용됨
- "공유된 이해(shared understanding)"가 핵심 가치

**Double Loop TDD**:
- 외부 인수 테스트 루프(시간~일, 사용자 관점) + 내부 유닛 테스트 루프(분, 개발자 관점)
- 하나의 인수 테스트가 통과할 때까지 다른 것을 시작하지 않음

**TCR (test && commit || revert)** — Kent Beck (2018):
- 테스트 통과 → 자동 커밋, 실패 → 자동 되돌림. "항상 green" 상태 유지
- 일상적 방법론이 아닌 학습용 실험으로 권장

> Sources:
> - [Introducing BDD - Dan North](https://dannorth.net/blog/introducing-bdd/)
> - [Given When Then - Martin Fowler](https://martinfowler.com/bliki/GivenWhenThen.html)
> - [test && commit || revert - Kent Beck](https://medium.com/@kentbeck_7670/test-commit-revert-870bbd756864)

---

## 3. 업계 채택 현황 및 근거

### 3.1 학술 근거

**CONSENSUS**

| 논문 | 핵심 발견 |
|------|----------|
| **Shull et al. (2010)** | 결함 밀도 **40~90% 감소** 가능. 단, 초기 개발 속도 **15~35% 감소**. 대부분 학생 대상 실험으로 산업 일반화에 제약 |
| **Fucci et al. (2017)** | test-first 자체는 코드 품질/생산성에 유의미한 영향 없음. **짧은 개발 사이클(fine granularity)**이 품질 향상의 핵심 요인. TDD 찬성론의 핵심 전제에 도전 |
| **Munir et al. (2014)** | 외부 품질(결함 감소)에 대체로 긍정적. 내부 품질/생산성은 결과 불일관(inconclusive). 산업 현장 연구 부족 |
| **Rafique & Misic (2013)** | 외부 코드 품질에 약간의 긍정적 효과. 생산성은 부정적~중립적. 학계 > 산업 현장 효과 |
| **Karac & Turhan (2018)** | "현재 근거로는 TDD가 품질/생산성을 확실히 개선한다고 단정할 수 없다." 가장 보수적인 메타리뷰 |
| **Nagappan et al. (2008)** | IBM/MS 팀 대상: 결함 밀도 **40~90% 감소**, 초기 개발 시간 **15~35% 증가** |

**핵심 수치 요약:**

| 지표 | 결과 | 출처 |
|------|------|------|
| 결함 밀도 감소 | 40~90% (최선 시나리오) | Shull et al., Nagappan et al. |
| 초기 개발 시간 증가 | 15~35% | 복수 연구 |
| Test-first vs Test-last 품질 차이 | 유의미하지 않음 | Fucci et al. (2017) |
| 생산성 영향 | 불확실/중립~부정적 | 대부분의 메타분석 |

> Sources:
> - [Fucci et al. (2017)](https://doi.org/10.1109/TSE.2016.2616877)
> - [Karac & Turhan (2018)](https://doi.org/10.1109/MS.2018.2801554)
> - [Nagappan et al. (2008)](https://doi.org/10.1007/s10664-008-9062-z)

---

### 3.2 채택률

**CONSENSUS**

| 분류 | 추정 비율 |
|------|----------|
| 엄격한 TDD (strict red-green-refactor) | 5~15% |
| 부분적 TDD (일부 상황에서 test-first) | 15~30% |
| 테스트를 나중에 작성 (test-after) | 40~50% |
| 테스트를 거의 작성하지 않음 | 15~25% |

- TDD에 대해 "좋은 실천 방법이다"라고 동의하는 비율과 **실제 실천 비율 사이에 큰 괴리** 존재
- JetBrains Developer Ecosystem Survey (2023): 단위 테스트 "항상 또는 자주" 작성 ~50~55%, 이 중 TDD는 ~10~20%

---

### 3.3 기업 사례

**CONSENSUS**

| 기업 | TDD 관행 |
|------|---------|
| **Google** | 엄격한 TDD 강제하지 않으나 높은 테스트 커버리지 + 자동화된 인프라 강조. "Testing on the Toilet" 프로그램으로 문화 전파 |
| **Microsoft** | Engineering Fundamentals에서 TDD 공식 권장. Red-Green-Refactor 예제 문서화. "테스트 없이 제출된 코드는 불완전한 것으로 간주" |
| **Pivotal/VMware Tanzu** | 가장 적극적인 TDD 실천 조직. XP 전사 채택, 페어 프로그래밍 + TDD가 기본 |
| **thoughtbot** | TDD를 "가장 중요한 XP 규칙"으로 실천. *Testing Rails* 출판, Upcase 코스 제공 |
| **ThoughtWorks** | 7대 테스팅 원칙에 TDD 포함. 2024 조사: TDD 팀이 비TDD 팀 대비 **32% 더 자주 릴리스** |

> Sources:
> - [Microsoft Engineering Playbook - TDD](https://microsoft.github.io/code-with-engineering-playbook/automated-testing/unit-testing/tdd-example/)
> - [thoughtbot Playbook - TDD](https://thoughtbot.com/playbook/developing/test-driven-development)
> - [ThoughtWorks - Seven Guiding Principles of Testing](https://www.thoughtworks.com/en-us/insights/blog/testing/seven-guiding-principles-testing)

---

### 3.4 "TDD is Dead" 논쟁 및 반대 의견

**CONSENSUS (주요 반대 의견)**

**DHH (2014) "TDD is Dead. Long Live Testing.":**
- TDD가 테스트 용이성을 위해 불필요한 간접층/의존성 주입을 강제하여 코드 설계를 왜곡
- 과도한 모킹이 실제 동작과 동떨어진 테스트를 만듦
- 통합 테스트가 단위 테스트보다 더 가치 있는 경우가 많다

**Kent Beck 응답**: TDD는 만능이 아니지만 불확실한 문제에서 확신(confidence)을 제공

**Martin Fowler 중재**: Self-Testing Code의 가치는 모두 동의. 차이는 test-first를 얼마나 엄격히 따를 것인가의 정도 문제

**Jim Coplien "Why Most Unit Testing is Waste":**
- 단위 테스트의 상당 부분이 ROI가 낮음
- 시스템 수준 테스트와 계약 테스트가 더 효과적

**커뮤니티 합의 (Emerging Consensus):**
1. **테스트 자체는 필수적** — 거의 모든 진영이 동의
2. **엄격한 TDD는 선택적** — 모든 상황에 적합하지 않음
3. **"Test-first vs Test-after"보다 "테스트를 하느냐 안 하느냐"가 더 중요한 분기점**
4. **"TDD는 도구이지 종교가 아니다"** — 가장 많이 반복되는 주장

> Sources:
> - [DHH - TDD is Dead](https://dhh.dk/2014/tdd-is-dead-long-live-testing.html)
> - [Is TDD Dead? 대화 시리즈 - Fowler, Beck, DHH](https://martinfowler.com/articles/is-tdd-dead/)

---

## 4. AI 시대의 TDD

### 4.1 학술 연구

**CONSENSUS**

| 연구 | 핵심 발견 |
|------|----------|
| **TiCoder (MS Research, 2024)** | 테스트 기반 의도 명확화로 pass@1 정확도 **+45.97%** 향상. 인지 부하 유의하게 감소 |
| **Mathews & Nagappan (2024)** | GPT-4/Llama 3에 테스트 케이스 포함 시 일관적으로 더 높은 문제 해결 성공률 |
| **WebApp1K (2025)** | TDD 성공의 핵심은 일반 코딩 능력이 아닌 **"지시 따르기(instruction following)"** 능력. 컨텍스트 길이가 주요 병목 |
| **Zietsman (2026)** | AI로 AI 코드를 리뷰하면 "같은 맹점 공유" → **실행 가능 명세(테스트)가 유일한 탈출구**. 훈련 데이터에 없는 도메인 관습은 어떤 모델도 탐지 불가 |

**핵심 정량 데이터:**

| 출처 | 핵심 수치 |
|------|-----------|
| TiCoder (MS Research) | pass@1 정확도 **+45.97%** |
| Wang et al. (2025) | 명세 기반 리뷰 → 개발자 채택률 **+90.9%** |
| ASOS TDVD | 4-6개월 예상 → **4주 완료** (7-10배 가속) |
| ThoughtWorks (2024) | TDD 팀이 **32% 더 자주** 릴리스 |
| DORA (2026) | AI 채택 → 처리량 + 불안정성 **동시 증가** |

> Sources:
> - [TiCoder (arXiv:2404.10100)](https://arxiv.org/abs/2404.10100)
> - [TDD for Code Generation (arXiv:2402.13521)](https://arxiv.org/abs/2402.13521)
> - [WebApp1K (arXiv:2505.09027)](https://arxiv.org/abs/2505.09027)
> - [The Specification as Quality Gate (arXiv:2603.25773)](https://arxiv.org/abs/2603.25773)

---

### 4.2 AI 도구별 TDD 지원

**CONSENSUS**

| 도구 | TDD 지원 수준 |
|------|-------------|
| **Claude Code** | 공식 권장: "Give Claude a way to verify its work" = 가장 높은 레버리지. Writer/Reviewer 패턴, PostToolUse 훅으로 자동 테스트 실행, 멀티에이전트 TDD (RED/GREEN/REFACTOR 분리) |
| **Cursor** | 공식 5단계 TDD 워크플로우 문서화. "에이전트는 명확한 반복 대상(테스트)이 있을 때 가장 잘 작동" |
| **GitHub Copilot** | Red-Green-Refactor 가속화 가이드 제공. 보일러플레이트를 Copilot이 처리 → 개발자는 테스트 로직에 집중 |
| **Windsurf** | Cascade Write Mode로 TDD 90% 자동화 |

**"테스트 먼저 → AI 구현" 패턴이 효과적인 이유:**
1. 테스트는 추상적 자연어보다 **구체적 예시 기반 명세** 역할 → LLM이 요구사항을 더 정확히 이해
2. 테스트가 "사용자 정의 가드레일"로 기능 → AI 환각 즉시 탐지
3. AI가 테스트 실행 → 실패 확인 → 수정 → 재실행을 **자율적으로 반복** 가능
4. 작은 단계로 분할하여 컨텍스트 윈도우 오염 방지
5. 매 단계 테스트로 검증 → 10개 동시 변경 디버깅이 아닌 1개 변경의 즉시 검증

**CONSENSUS (AI 시대에 TDD가 더 중요해지는가: 예, 그러나 역할이 변했다)**

- **전통적 TDD**: 개발자가 테스트 작성 + 개발자가 구현 (설계 도구)
- **AI 시대 TDD**: **인간이 테스트(명세) 작성 + AI가 구현** (검증 및 명세 도구)
- ThoughtWorks Technology Radar (Vol.32-33): "TDD와 Static Analysis 등 **기존 관행 강화가 AI 시대에 더 중요**"
- 756,000 AI 코드 샘플 중 약 20%가 존재하지 않는 패키지 추천 → 테스트가 자동 탐지

**DECISION NEEDED**

- AI + TDD 워크플로우를 어떤 패턴으로 표준화할지 (기본 TDD+AI vs 멀티에이전트 TDD vs TDVD)
- 테스트와 코드를 동일 AI에게 동시에 작성시키는 것을 금지할지 (순환적 검증 위험)

> Sources:
> - [Claude Code Best Practices](https://code.claude.com/docs/en/best-practices)
> - [Cursor Agent Best Practices](https://cursor.com/blog/agent-best-practices)
> - [GitHub Blog: TDD with Copilot](https://github.blog/ai-and-ml/github-copilot/github-for-beginners-test-driven-development-tdd-with-github-copilot/)
> - [Why TDD Works Well with AI (Codemanship, 2026)](https://codemanship.wordpress.com/2026/01/09/why-does-test-driven-development-work-so-well-in-ai-assisted-programming/)
> - [ASOS: Test-Driven Vibe Development](https://medium.com/asos-techblog/introducing-test-driven-vibe-development-0effe6430691)
> - [Forcing Claude Code to TDD (alexop.dev)](https://alexop.dev/posts/custom-tdd-workflow-claude-code-vue/)

---

### 4.3 AI + TDD 실전 워크플로우 패턴

**CONSENSUS**

**패턴 A: 기본 TDD + AI (모든 도구 호환)**
```
1. 인간: 요구사항 정의 → 실패하는 테스트 작성
2. AI: 테스트를 통과하는 최소 코드 구현 (반복)
3. 인간: 코드 리뷰 → 추가 엣지 케이스 테스트
4. AI: 리팩토링 (테스트가 안전망)
5. 커밋
```

**패턴 B: 멀티에이전트 TDD (Claude Code)**
```
1. RED 에이전트: 실패하는 테스트 작성 (구현 컨텍스트 격리)
2. GREEN 에이전트: 최소 구현 (테스트만 보고 구현)
3. REFACTOR 에이전트: 코드 품질 개선 (테스트 통과 유지)
4. 훅(hook)으로 워크플로우 자동 강제 → 스킬 활성화율 ~20% → ~84%로 향상
```

**패턴 C: Spec-Driven + TDD 하이브리드 (Anthropic/ThoughtWorks 권장)**
```
1. Constitution/Spec 작성 (프로젝트 원칙 + 요구사항 명세)
2. Plan 작성 (구현 계획)
3. 수용 테스트 작성 (명세 기반)
4. AI가 구현 + 자동 테스트 반복
5. 인간 리뷰 + 커밋
```

**공통 베스트 프랙티스:**
- AI에게 "TDD를 하고 있다"고 **명시적으로 알릴 것** — 기본적으로 구현 우선 모드
- 테스트와 코드를 **동일 AI에게 동시에 작성시키지 말 것** — 순환적 검증 위험
- 테스트를 **별도 커밋으로 먼저 저장** — 구현 중 테스트 변경 방지
- CLAUDE.md/Cursor Rules에 **테스트 컨벤션 기록** — 일관된 패턴 유지

> Sources:
> - [TDD with Claude Code (Steve Kinney)](https://stevekinney.com/courses/ai-development/test-driven-development-with-claude)
> - [Superpowers Framework](https://github.com/obra/superpowers)
> - [Claude Code Ultimate Guide - TDD section](https://github.com/FlorianBruniaux/claude-code-ultimate-guide/blob/main/guide/workflows/tdd-with-claude.md)

---

## 5. 컨텍스트별 TDD 적용 가이드

### 5.1 백엔드 API

**CONSENSUS**

- **TDD가 가장 효과적인 영역.** 명확한 입출력, 제어 가능한 상태, 순수 비즈니스 로직 비중이 높음
- **Controller 레이어**: HTTP 요청/응답 시뮬레이션하며 TDD. 입력 검증, 상태 코드, 응답 스키마를 먼저 테스트로 정의
- **Service 레이어 (비즈니스 로직)**: TDD의 핵심 대상. "Functional Core, Imperative Shell" 패턴 적용 권장
- **Repository 레이어**: 실제 DB를 사용하는 통합 테스트가 더 효과적. DB를 모킹하면 리스크 커버리지가 크게 감소
- **권장 전략**: Testing Diamond 모델 — API 수준 컴포넌트 테스트 + 복잡한 로직에만 유닛 TDD

> Sources:
> - [goldbergyoni/nodejs-testing-best-practices](https://github.com/goldbergyoni/nodejs-testing-best-practices)
> - [Google Testing Blog - Functional Core, Imperative Shell (2025.10)](https://testing.googleblog.com/)

---

### 5.2 프론트엔드

**CONSENSUS**

- TDD 적용 가능하지만 **전통적 백엔드 TDD와 다른 접근 필요**
- **Kent C. Dodds의 접근법 (사실상 표준)**: "테스트가 소프트웨어 사용 방식과 유사할수록 더 많은 자신감 제공." Testing Trophy 모델 (Integration 최대 비중)
- **효과적인 영역**: 커스텀 훅, 상태 관리 로직, 유틸리티 함수, API 통합 레이어
- **비효과적인 영역**: 순수 시각적 컴포넌트, 레이아웃, 애니메이션 → 시각적 회귀 테스트(Applitools, Percy)가 더 적합
- 구현 세부사항(state 변수, 이벤트 핸들러) 테스트 금지 — **사용자가 보는 관찰 가능한 효과만 테스트**

**DECISION NEEDED**

- 프론트엔드에서 TDD 적용 범위를 어디까지로 권장할지 (커스텀 훅/상태 관리만 vs 컴포넌트까지)

> Sources:
> - [Kent C. Dodds - Write Tests](https://kentcdodds.com/blog/write-tests)
> - [Kent C. Dodds - Testing Trophy](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications)

---

### 5.3 레거시 코드

**CONSENSUS**

레거시 코드 = "테스트가 없는 코드" (Michael Feathers). 핵심 딜레마: 코드 변경에 테스트가 필요하지만, 테스트 추가에 코드 변경이 필요.

**4대 기법:**

| 기법 | 설명 |
|------|------|
| **Characterization Test** | 현재 동작을 스냅샷으로 캡처. "코드가 무엇을 해야 하는지가 아닌, 실제로 무엇을 하는지 테스트" |
| **Seam** | 코드를 변경하지 않고 동작을 바꿀 수 있는 지점 활용. 클래스 상속으로 문제 동작 오버라이드 |
| **Sprout Method/Class** | 새 코드를 별도의 테스트 가능한 메서드/클래스로 분리. 기존 코드에 최소 변경 |
| **Wrap Method/Class** | 원래 메서드를 래핑하여 새 로직 추가. 기존 메서드는 보존 |

**TDD 도입 프로세스**: Characterization Test → 새 기능은 TDD로 Sprout/Wrap → 점진적 커버리지 확대

> Sources:
> - [Key Points of Working Effectively with Legacy Code](https://understandlegacycode.com/blog/key-points-of-working-effectively-with-legacy-code/)
> - [Michael Feathers - Characterization Testing](https://michaelfeathers.silvrback.com/characterization-testing)

---

### 5.4 마이크로서비스

**CONSENSUS**

- **Contract Testing이 마이크로서비스의 TDD.** Consumer-Driven Contract Testing (CDC) + TDD 워크플로우
- **이중 사이클**: 짧은 사이클(서비스 내 유닛/통합 TDD) + 긴 사이클(팀 간 계약 설계)
- Spotify Honeycomb 모델: 유닛 테스트 최소화, **통합 테스트에 최대 비중**

> Sources:
> - [Pact Docs](https://docs.pact.io/)
> - [Microsoft - Contract Testing](https://devblogs.microsoft.com/ise/pact-contract-testing-because-not-everything-needs-full-integration-tests/)

---

### 5.5 TDD 적용 결정 매트릭스

**CONSENSUS**

**TDD 강력 권장:**
- 순수 비즈니스 로직/알고리즘
- 요구사항이 명확한 경우
- 코드 예상 수명 1년 이상
- 보안/금융/의료 등 결함 비용이 높은 도메인
- 오픈소스 라이브러리
- 레거시 코드 리팩토링 (Characterization Test + Sprout/Wrap)

**TDD 비권장 — 대안 사용:**
- 프로토타입/탐색적 코딩 → 나중에 테스트 추가
- 순수 UI/시각적 요소 → 시각적 회귀 테스트
- 단명 코드/일회성 스크립트 → 테스트 최소화
- I/O 중심 배관 코드 → 통합 테스트
- CRUD 보일러플레이트 → 컴포넌트 테스트

**"Functional Core, Imperative Shell" 프레임워크** (가장 실용적인 판단 기준):
- **Functional Core** (순수 함수, 비즈니스 로직) → **TDD 강력 권장**. 모킹 불필요, 빠르고 결정적
- **Imperative Shell** (I/O, DB, 외부 서비스) → **통합 테스트**. TDD 대신 기능 구현 후 테스트 작성이 현실적

> Sources:
> - [Functional Core, Imperative Shell](https://kennethlange.com/functional-core-imperative-shell/)
> - [Google Testing Blog (2025.10)](https://testing.googleblog.com/)

---

## 6. 팀 도입 전략

### 6.1 강제 vs 권장

**CONSENSUS**

- **합의: 권장이 강제보다 효과적**
- TDD를 의무화하면 형식적으로만 따르고 실질적 이점을 얻지 못함
- **"PR에 테스트 포함 필수" 규칙**이 TDD 강제보다 실효성 높음 (~60-70% 채택)
- "의무화가 요점을 놓친다. 금지도 마찬가지다. 진짜 질문은 팀에 실험할 공간이 있는가이다."

**DECISION NEEDED**

- "PR에 테스트 포함 필수" 규칙만 적용할지, TDD를 특정 상황에서 권장할지

---

### 6.2 점진적 도입 프로세스

**CONSENSUS**

| 단계 | 활동 | 기간 |
|------|------|------|
| 1단계: 파일럿 | 2-3명 자발적 지원자로 새 기능에 TDD 적용 | 1-2개월 |
| 2단계: 학습 기반 | Coding Dojo (주 1회), 페어 프로그래밍, 독서 워크숍 병행 | 2-4개월 |
| 3단계: 경험 확산 | 파일럿 팀원이 다른 팀에 멘토링/페어링으로 확산 | 3-6개월 |
| 4단계: 표준화 | 팀 TDD 관행 합의, ADR로 문서화 | 6개월+ |

**생산성 타임라인:**
- 학습 단계: **2-4개월**, 생산성 15-33% 감소
- TDD 숙련까지: **3-4개월**의 꾸준한 연습으로 시간 절약 효과 발생
- 6개월 후: 초보 개발자도 TDD 실천 유지 확인 (ScienceDirect 종단 연구)

> Sources:
> - [InfoQ - Making TDD Stick](https://www.infoq.com/articles/levison-TDD-adoption-strategy/)
> - [Should TDD Be Mandatory?](https://craftingtechteams.substack.com/p/should-tdd-be-mandatory-why-are-so)

---

### 6.3 학습 방법

**CONSENSUS**

| 방법 | 설명 |
|------|------|
| **Coding Dojo** | 15명까지, 5-10분마다 드라이버 교대. 테스트 통과 시만 관객 피드백. 안전한 학습 환경 조성이 핵심 |
| **Code Kata** | 15분 일일 연습. String Calculator Kata (Roy Osherove), kata-log.rocks/tdd 참조 |
| **Code Retreat** | 하루 종일 같은 문제를 다양한 제약 조건으로 반복 |
| **페어/몹 프로그래밍** | "TDD를 채택한 몹은 항상 올바른 일을 하도록 고집한다" |
| **독서 워크숍** | 월 1-2장씩 토론 |

**인기 TDD Kata:**

| Kata | 난이도 | 학습 목표 |
|------|--------|----------|
| FizzBuzz | 초급 | TDD 입문, Red-Green-Refactor 리듬 |
| String Calculator | 초급~중급 | 점진적 요구사항 추가 |
| Bowling Game | 중급 | 상태 관리 |
| Gilded Rose | 중급~고급 | 리팩토링, 레거시 코드 이해 |

**핵심 서적:**

| 제목 | 저자 | 핵심 |
|------|------|------|
| *TDD by Example* | Kent Beck | TDD 원본, Red-Green-Refactor |
| *Growing OO Software, Guided by Tests* | Freeman & Pryce | London School TDD, Double Loop |
| *Working Effectively with Legacy Code* | Michael Feathers | 레거시 코드에 TDD 도입 |
| *xUnit Test Patterns* | Gerard Meszaros | Test Double 분류, 안티패턴 |

> Sources:
> - [Kata-Log: TDD Katas](https://kata-log.rocks/tdd)
> - [TDD MOOC](https://tdd.mooc.fi/1-tdd/)
> - [thoughtbot Upcase - Fundamentals of TDD](https://thoughtbot.com/upcase/fundamentals-of-tdd)

---

## 7. TDD 패턴 및 안티패턴

### 7.1 Transformation Priority Premise (TPP)

**CONSENSUS**

Uncle Bob이 제안한 Green 단계 구현 변환 우선순위. 더 단순한(위쪽) 변환을 우선 선택하면 교착 상태(impasse)를 방지.

| 순위 | 변환 | 설명 |
|------|------|------|
| 1 | {} → nil | 코드 없음 → nil |
| 2 | nil → constant | nil → 상수값 |
| 3 | constant → constant+ | 단순 → 복잡한 상수 |
| 4 | constant → scalar | 상수 → 변수/인자 |
| 5 | statement → statements | 무조건적 문장 추가 |
| 6 | unconditional → if | 조건문으로 경로 분리 |
| 7 | scalar → array | 스칼라 → 배열 |
| 8 | array → container | 배열 → 컨테이너(맵 등) |
| 9-10 | recursion / if → while | 재귀 또는 반복 |
| 11-14 | expression → function 등 | 고급 변환 |

> Sources:
> - [TPP - Uncle Bob](http://blog.cleancoder.com/uncle-bob/2013/05/27/TheTransformationPriorityPremise.html)

---

### 7.2 Test Double 선택 가이드

**CONSENSUS**

| 유형 | 정의 | 검증 방식 | 예시 |
|------|------|----------|------|
| **Dummy** | 전달만, 실제 사용 안 됨 | 없음 | null 객체 |
| **Fake** | 동작하는 축약 구현 | 상태 검증 | 인메모리 DB |
| **Stub** | 미리 정해진 응답만 반환 | 상태 검증 | `when().thenReturn()` |
| **Spy** | Stub + 호출 정보 기록 | 상태 + 행위 | 호출 횟수/인자 기록 |
| **Mock** | 기대하는 호출을 미리 프로그래밍 | 행위 검증 | `verify().send()` |

**선택 의사결정:**
- 의존성이 단순하고 빠른가? → **실제 객체** 사용
- 외부 서비스(API, DB, 파일)? → **Fake 또는 Stub**
- 부작용 검증 필요(이메일, 로깅)? → **Spy 또는 Mock**
- **핵심 원칙**: "Don't mock what you don't own" — 외부 라이브러리를 직접 mock하지 말고 Adapter를 만들어 mock

> Sources:
> - [Martin Fowler - Test Double](https://martinfowler.com/bliki/TestDouble.html)
> - [testdouble/contributing-tests Wiki](https://github.com/testdouble/contributing-tests/wiki/Test-Double)

---

### 7.3 TDD 안티패턴

**CONSENSUS**

**테스트 코드 안티패턴 (James Carr + 확장):**

| 안티패턴 | 설명 |
|---------|------|
| **The Liar** | 모든 케이스 통과하지만 의미 있는 검증 없음. `expect(result).toBeDefined()`만 사용 |
| **The Giant** | 수천 줄의 거대한 테스트. God Object 반영 |
| **The Mockery** | mock이 너무 많아 실제 시스템이 아닌 mock 데이터를 테스트 |
| **The Inspector** | 캡슐화 위반하여 내부 구현 직접 테스트. 리팩토링 시 깨짐 |
| **Excessive Setup** | 수백 줄의 setup 코드로 테스트 목적 불명확 |
| **The Slow Poke** | 불필요하게 느린 테스트. 개발자가 실행 회피 |
| **The Local Hero** | 특정 개발 환경에서만 통과 |
| **Generous Leftovers** | 한 테스트가 생성한 데이터를 다른 테스트가 재사용. 실행 순서 의존 |
| **Second Class Citizen** | 테스트 코드에 설계 원칙 미적용. 중복, 나쁜 구조 |

**TDD 프로세스 안티패턴:**

| 안티패턴 | 올바른 접근 |
|---------|-----------|
| 리팩토링 단계 생략 | 리팩토링은 신성한(sacred) 단계. 반드시 실행 |
| 구현 테스트 (행위 아닌) | 행위(behavior)를 테스트. 구현은 변해도 행위는 안정적 |
| 구현 전 테스트 대량 작성 | 한 번에 하나의 테스트만 작성하고 통과 |
| Gold Plating (과잉 테스트) | 100% 커버리지 맹목 추구 대신 critical path 커버리지 |
| TDD를 종교처럼 따르기 | 도구로 사용하되 맥락에 맞게 적용 |

> Sources:
> - [James Carr TDD Anti-Patterns](https://bryanwilhite.github.io/the-funky-knowledge-base/entry/kb2076072213/)
> - [yegor256 - Unit Testing Anti-Patterns](https://www.yegor256.com/2018/12/11/unit-testing-anti-patterns.html)
> - [Codepipes - Software Testing Anti-Patterns](https://blog.codepipes.com/testing/software-testing-antipatterns.html)

---

### 7.4 GWT vs AAA 비교

**CONSENSUS**

| 항목 | Given-When-Then (GWT) | Arrange-Act-Assert (AAA) |
|------|----------------------|--------------------------|
| 기원 | BDD (Dan North, 2003) | XP (Bill Wake) |
| 사고방식 | 행위 중심 | 기술적 상태 중심 |
| 가독성 | 비개발자도 읽을 수 있음 | 개발자 중심 |
| 도구 | Cucumber, SpecFlow, Behave | xUnit, JUnit, pytest, Jest |

**선택 가이드:**
- 단위 테스트, 내부 로직 → **AAA**
- 인수 테스트, 이해관계자 공유 → **GWT**
- BDD 도구 사용 → **GWT**
- React/Vue 컴포넌트 테스트 → **AAA**

> Sources:
> - [Martin Fowler - GivenWhenThen](https://martinfowler.com/bliki/GivenWhenThen.html)
> - [QWAN - TDD Heuristics: GWT or AAA](https://www.qwan.eu/2021/09/02/tdd-given-when-then.html)

---

### 7.5 고급 TDD 기법

**CONSENSUS**

| 기법 | 설명 | TDD와의 관계 |
|------|------|-------------|
| **Property-Based Testing** | 속성 정의 → 프레임워크가 수천 개 임의 입력 자동 생성. 도구: fast-check(JS), Hypothesis(Python), QuickCheck(Haskell) | 예제 기반 TDD로 구현 후, property 테스트 추가로 엣지 케이스 발견 |
| **Approval/Snapshot Testing** | 출력의 "황금 마스터" 스냅샷 저장 후 비교. 도구: Jest Snapshot, ApprovalTests | 레거시 코드 특성화 테스트에 적합. 정통 TDD의 대안이지 대체가 아님 |
| **Contract Testing** | Consumer가 Provider에 기대하는 상호작용을 계약으로 정의. 도구: Pact | 마이크로서비스의 TDD. E2E 테스트의 느리고 취약한 문제 해결 |
| **Mutation Testing** | 코드에 의도적 변이 삽입, 테스트가 감지하는지 확인. 도구: Stryker(JS), mutmut(Python), PIT(Java) | TDD로 작성된 테스트의 품질을 사후 검증하는 메타 테스트 |

> Sources:
> - [fast-check GitHub](https://github.com/dubzzz/fast-check)
> - [Hypothesis - What is PBT](https://hypothesis.works/articles/what-is-property-based-testing/)
> - [Stryker Mutator](https://stryker-mutator.io/)
> - [Pact Docs](https://docs.pact.io/)

---

## 종합: DECISION NEEDED 목록

| # | 결정 사항 | 맥락 |
|---|----------|------|
| 1 | 팀 기본 TDD 스타일 (Classical vs London) | Fowler는 Classical 선호, 상황별 혼합이 실무 권장 |
| 2 | AI + TDD 워크플로우 표준 패턴 | 기본 TDD+AI vs 멀티에이전트 TDD vs TDVD |
| 3 | 테스트와 코드를 동일 AI에게 동시 작성 허용 여부 | 순환적 검증 위험 vs 편의성 |
| 4 | 프론트엔드 TDD 적용 범위 | 훅/상태 관리만 vs 컴포넌트까지 |
| 5 | TDD 의무화 수준 | "PR에 테스트 포함 필수"만 vs 특정 상황 TDD 권장 |
| 6 | TDD 학습 투자 수준 | Coding Dojo 주 1회 + 페어 프로그래밍 or 자율 학습 |
