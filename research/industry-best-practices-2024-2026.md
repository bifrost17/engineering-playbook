# Industry Best Practices Research (2024-2026)

> 작성일: 2026-04-07
> 목적: Engineering Playbook 문서 작성을 위한 업계 베스트 프랙티스 리서치

---

## 1. AI-Assisted Development Best Practices

### 1.1 CLAUDE.md 작성 가이드

**CONSENSUS (업계 합의)**

- **간결함이 핵심**: 200줄 이하 권장, 최대 300줄 넘지 않기. 매 세션마다 컨텍스트에 로드되므로 비용 직결 ([Anthropic 공식](https://code.claude.com/docs/en/best-practices))
- **Progressive Disclosure 패턴**: 루트 CLAUDE.md는 전역 규칙만, 하위 디렉토리 CLAUDE.md는 해당 디렉토리 작업 시 자동 로드
- **코드처럼 관리**: git에 커밋, 팀원과 공유, 정기 리뷰 및 프루닝
- **포함해야 할 것**: AI가 추론할 수 없는 빌드 명령어, 기본값과 다른 코드 스타일, 테스트 방법, 브랜치 네이밍, 아키텍처 결정, 환경 변수 quirks
- **제외해야 할 것**: 코드에서 읽을 수 있는 것, 언어 표준 컨벤션, 상세 API 문서(링크로 대체), 자주 변하는 정보, 파일별 설명
- **강조 표시**: 중요한 규칙은 "IMPORTANT", "YOU MUST" 등으로 강조하여 준수율 향상

**DECISION NEEDED (팀 결정 필요)**

- CLAUDE.md 외에 `.claude/skills/`로 도메인별 스킬 분리할지 여부
- `CLAUDE.local.md` (개인 설정)의 사용 범위

> Sources:
> - [Anthropic 공식 Best Practices](https://code.claude.com/docs/en/best-practices)
> - [HumanLayer - Writing a good CLAUDE.md](https://www.humanlayer.dev/blog/writing-a-good-claude-md)
> - [Builder.io - 50 Claude Code Tips](https://www.builder.io/blog/claude-code-tips-best-practices)

---

### 1.2 AGENTS.md 작성 가이드

**CONSENSUS**

- **표준 마크다운, 필수 구조 없음**: 프로젝트에 맞게 자유 형식. Linux Foundation 산하 Agentic AI Foundation이 관리
- **다중 파일 지원**: 디렉토리마다 AGENTS.md 배치 가능, 가장 가까운 파일이 우선
- **권장 섹션**: Project Overview, Build & Test Commands, Code Style, Testing Instructions, Contribution Guidelines
- **작성 팁**: 설치/빌드 명령어를 맨 위에, H2 섹션으로 분류, 불릿 포맷, 명령어는 백틱, 파일 경로 명시

**CONSENSUS (ETH Zurich 경고)**

- **"80% 삭제" 규칙**: 일반적인 AGENTS.md 내용의 80%는 불필요. 에이전트가 `ls`, `find`로 발견 가능한 정보는 삭제
- **과도한 컨텍스트는 성능 저하 + 비용 20% 증가**: "Lost in the Middle" 현상으로 핵심 정보를 놓침
- **유지할 것**: 금지 라이브러리, 비자명 빌드 시퀀스, 모호한 기술 선택 (예: Pandas 대신 Polars)
- **삭제할 것**: 파일 트리, 프로젝트 요약, 장황한 스타일 문서 (모델은 기존 코드 패턴을 모방)

**DECISION NEEDED**

- CLAUDE.md vs AGENTS.md 중 어느 것을 주력으로 사용할지 (또는 둘 다)
- AGENTS.md 파일 수의 적정 기준 (OpenAI는 88개 사용)

> Sources:
> - [AGENTS.md 공식 사이트](https://agents.md/)
> - [OpenAI Codex - AGENTS.md Guide](https://developers.openai.com/codex/guides/agents-md)
> - [ETH Zurich Study - MarkTechPost](https://www.marktechpost.com/2026/02/25/new-eth-zurich-study-proves-your-ai-coding-agents-are-failing-because-your-agents-md-files-are-too-detailed/)
> - [i-scoop - ETH Zurich AGENTS.md 분석](https://www.i-scoop.eu/agents-md/)

---

### 1.3 Context Engineering 원칙

**CONSENSUS**

핵심 정의: "모델이 보는 것을 큐레이션하여 더 나은 결과를 얻는 것" (Martin Fowler)

**4가지 전략 패턴 (Addy Osmani)**:

| 패턴 | 설명 |
|------|------|
| **Write** | 명확한 지시문/프롬프트 작성 |
| **Select** | 관련 컨텍스트만 선별 |
| **Compress** | 정보를 압축하여 토큰 절약 |
| **Isolate** | 서브에이전트로 탐색 격리 |

**Martin Fowler의 3가지 컨텍스트 유형**:
- **Instructions**: 작업 수행 방법 지시 (테스트 컨벤션 등)
- **Guidance**: 일반적 가이드라인/가드레일
- **Skills**: 필요 시 lazy-loading되는 문서/스크립트

**컨텍스트 주입 결정권자 3가지**:
1. LLM 자율 판단 - 자동이나 불확실성 존재
2. 사람이 트리거 - 제어 가능하나 자동화 감소
3. 소프트웨어 결정 - 결정론적이나 유연성 감소

**Spotify의 6가지 프롬프트 설계 원칙**:
1. 에이전트 특성에 맞춤 (Claude Code는 "최종 상태" 서술이 효과적)
2. 전제 조건 명시 (언제 행동하지 말아야 하는지)
3. 구체적 코드 예시 포함
4. 검증 가능한 목표 정의 (테스트로 측정)
5. 변경 사항 격리 (복잡한 마이그레이션 → 단일 작업으로 분할)
6. 피드백 반복

**Spotify의 도구 제한 전략**: 코드 검색/문서 도구를 의도적으로 제외하고, verify/git/bash(allowlist)만 제공. "비예측성의 차원"을 줄여 예측 가능성 향상

**DECISION NEEDED**

- 컨텍스트 주입을 LLM 자율 vs 소프트웨어 결정 중 어디에 비중을 둘지
- Skills 파일의 조직 구조

> Sources:
> - [Martin Fowler - Context Engineering for Coding Agents](https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html)
> - [Spotify - Background Coding Agents: Context Engineering](https://engineering.atspotify.com/2025/11/context-engineering-background-coding-agents-part-2)
> - [Addy Osmani - Mastering Context Engineering](https://www.linkedin.com/posts/addyosmani_ai-programming-softwareengineering-activity-7372154521310932992-c71q)

---

### 1.4 Spec-Driven Development (GitHub Spec Kit)

**CONSENSUS**

핵심 개념: 스펙이 코드를 "생성"하는 1차 아티팩트. 코딩 에이전트를 "문자 그대로 이해하는 페어 프로그래머"로 취급.

**3단계 워크플로우**:

| 단계 | 명령어 | 산출물 |
|------|--------|--------|
| **Specify** | `/speckit.specify` | `spec.md` (유저 스토리, 수용 기준) |
| **Plan** | `/speckit.plan` | `plan.md`, `data-model.md`, `contracts/` |
| **Tasks** | `/speckit.tasks` | `tasks.md` (실행 가능한 작업 목록) |

**Constitution (헌법)**: 9개 불변 원칙
- Library-first: 모든 기능은 독립 라이브러리로 시작
- Test-first: **테스트 없이 코드 작성 금지**
- Simplicity: 최대 3개 프로젝트, 미래 대비 금지
- Integration-first: Mock 대신 실제 DB/서비스

**2025-2026 생태계**: Spec Kit(GitHub), Kiro(AWS, IDE 내장), Tessl 등 SDD 도구 다양화

**DECISION NEEDED**

- Spec Kit 도입 여부 (vs 자체 스펙 템플릿)
- Constitution 원칙을 팀에 맞게 커스터마이징
- SDD 적용 범위 (모든 기능 vs 중대 변경만)

> Sources:
> - [GitHub Spec Kit](https://github.com/github/spec-kit)
> - [GitHub Blog - Spec-Driven Development](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/)
> - [Martin Fowler - SDD 도구 비교](https://martinfowler.com/articles/exploring-gen-ai/sdd-3-tools.html)

---

### 1.5 AI Code Review Practices

**CONSENSUS**

- **AI = 기계적 검사, 인간 = 설계/아키텍처/비즈니스 로직**: 역할 분리가 핵심
- **통계**: 2026년 초 커밋의 41%가 AI 보조, 84% 개발자가 AI 도구 채택, 그러나 46%는 AI 정확성을 불신
- **품질 가드레일 필수**: AI 보조 없이 가드레일 없으면 6개월 내 버그 밀도 35-40% 증가
- **PR 크기 제한**: 500줄 이하 PR에서 30-40% 사이클 타임 개선, 1000줄 이상은 모델 성능 급락
- **린팅/포매팅은 CI에서**: 사소한 피드백이 사람에게 도달하지 않도록 자동화

**DECISION NEEDED**

- AI 코드 리뷰 도구 선택 (CodeRabbit, Qodo, GitHub Copilot 등)
- AI 리뷰를 필수 단계로 둘지, 선택 사항으로 둘지
- AI가 자동 approve 가능한 범위 (포매팅 only? 테스트 추가?)

> Sources:
> - [Verdent - Best AI for Code Review 2026](https://www.verdent.ai/guides/best-ai-for-code-review-2026)
> - [Qodo - AI Code Review Predictions 2026](https://www.qodo.ai/blog/5-ai-code-review-pattern-predictions-in-2026/)
> - [CodeAnt - Complete Code Review Process 2026](https://www.codeant.ai/blogs/good-code-review-practices-guide)

---

### 1.6 AI 사용 시점 vs 비사용 시점

**CONSENSUS**

| AI가 효과적인 경우 | AI에 의존하면 안 되는 경우 |
|---|---|
| 보일러플레이트 생성 | 아키텍처 결정 |
| 리팩토링/마이그레이션 | 보안 관련 로직 (반드시 인간 검증) |
| 테스트 작성 | 비즈니스 로직 설계 |
| 코드베이스 탐색/온보딩 | 성능 크리티컬 최적화 |
| 문서 초안 작성 | 법적/규제 준수 코드 |
| 버그 재현/디버깅 | 새로운 알고리즘 설계 |
| 코드 리뷰 기계적 체크 | 최종 의사결정 |

---

## 2. Developer Experience (DX) Best Practices

### 2.1 온보딩 체크리스트

**CONSENSUS**

**30-60-90일 프레임워크**:

| 기간 | 목표 |
|------|------|
| **Pre-boarding (D-7~D-1)** | 하드웨어 주문, 계정 생성, 문서 공유, 환경 사전 설정 |
| **Week 1-2 (30일)** | 환경 셋업, 아키텍처 개요(2페이지), 시스템 다이어그램, 팀 용어집, 첫 PR 제출 |
| **Month 2 (60일)** | 독립적 기능 개발, 코드 리뷰 참여, 온콜 쉐도잉 |
| **Month 3 (90일)** | 자율적 작업, 온보딩 프로세스 피드백 제공 |

**핵심 요소**:
- **버디 시스템**: 경험자를 기술 멘토 + 문화 가이드로 배정
- **문서 과잉 방지**: 2페이지 아키텍처 개요 + 시각적 다이어그램 + 팀 용어집으로 제한
- **검증 가능한 마일스톤**: "첫 PR 머지"가 D+5~D+10의 핵심 목표
- **효과**: 구조적 온보딩 시 신규 인력 유지율 50%↑, 생산성 62%↑

**DECISION NEEDED**

- 온보딩 체크리스트 관리 도구 (Notion, GitHub Issues, 자체 포털)
- 버디 배정 기준 (같은 팀 vs 다른 팀)

> Sources:
> - [Cortex - Developer Onboarding Guide](https://www.cortex.io/post/developer-onboarding-guide)
> - [DocuWriter - Developer Onboarding Best Practices](https://www.docuwriter.ai/posts/developer-onboarding-best-practices)

---

### 2.2 로컬 개발 환경 셋업

**CONSENSUS**

- **컨테이너화**: Docker/Docker Compose로 환경 일관성 보장, "works on my machine" 문제 해결
- **설정 파일 버전 관리**: `.env.example`, `docker-compose.yml`, `Makefile` 등을 리포에 포함
- **원커맨드 셋업**: `make setup` 또는 `./scripts/setup.sh`로 전체 환경 구축 가능해야 함
- **프로덕션 패리티**: 로컬 환경이 프로덕션과 최대한 유사해야 함
- **문서화**: README에 셋업 절차 명시, 트러블슈팅 섹션 포함

**DECISION NEEDED**

- Docker vs devcontainer vs Nix 중 표준화 방식
- 로컬 DB 사용 vs 공유 개발 DB
- 가상화 레벨 (전체 Docker vs 네이티브 + Docker DB만)

> Sources:
> - [Speedscale - Dev Environment Guide](https://speedscale.com/blog/the-ultimate-guide-to-a-smooth-dev-environment-setup-tips-and-best-practices/)
> - [GitHub - Optimize local dev environments](https://github.com/readme/guides/developer-onboarding)

---

### 2.3 Documentation-as-Code

**CONSENSUS**

- **코드와 같은 리포에 저장**: 같은 PR에서 코드 + 문서 변경
- **버전 관리**: 문서도 Git으로 변경 추적, 리뷰, 롤백
- **CI/CD 파이프라인**: 린팅(Vale), 링크 검증, 자동 배포
- **기능 완료 = 문서 완료**: 문서 없이 기능 완료로 인정하지 않음
- **단일 소스**: 중복 제거, 하나의 위치에서 관리
- **도구**: Markdown + SSG(MkDocs, Docusaurus), API 문서는 코드에서 자동 생성

**DECISION NEEDED**

- SSG 도구 선택 (MkDocs Material vs Docusaurus vs GitBook)
- 문서 린팅 도구 (Vale 등) 도입 여부
- API 문서 자동 생성 범위

> Sources:
> - [Write the Docs - Docs as Code](https://www.writethedocs.org/guide/docs-as-code/)
> - [Kong - What is Docs as Code](https://konghq.com/blog/learning-center/what-is-docs-as-code)

---

### 2.4 Inner Source

**CONSENSUS**

InnerSource = 오픈소스 개발 방법론을 사내 소프트웨어에 적용

**핵심 원칙**:
- **오픈 커뮤니케이션**: 모든 소통은 사내 공개, 서면, 아카이브, 완전해야 함
- **기여 기반 평가**: 기여의 가치로 판단 (meritocratic)
- **표준 문서**: README.md, CONTRIBUTING.md로 셀프서비스 기여 가능
- **코드 리뷰 분리**: Contributor와 Committer(Trusted Committer) 역할 구분
- **InnerSource Patterns**: InnerSource Commons에서 패턴 라이브러리 관리

**DECISION NEEDED**

- Inner Source 적용 범위 (전사 vs 특정 프로젝트)
- Trusted Committer 선정 기준
- 기여 가이드라인 수준

> Sources:
> - [InnerSource Commons](https://innersourcecommons.org/)
> - [InnerSource Patterns](https://patterns.innersourcecommons.org/)

---

### 2.5 Developer Portal (Backstage 등)

**CONSENSUS**

- **Backstage가 업계 표준**: 89% 시장 점유율, 3,400+ 조직, 200만+ 개발자 사용 (2026.01 기준)
- **핵심 기능**: Software Catalog (서비스 등록/관리), TechDocs (Markdown 기반), Software Templates (표준화), RBAC
- **TechDocs = 가장 인기 기능**: Spotify 내부에서 가장 많이 사용, "docs like code" 구현

**DECISION NEEDED**

- Backstage 도입 vs 간소화된 대안 (Port, Cortex)
- 자체 호스팅 vs 관리형 서비스 (Roadie 등) - 자체 호스팅은 전담 인력 필요
- 도입 시점 (팀 규모가 작을 때는 오버엔지니어링일 수 있음)

> Sources:
> - [Backstage Wrapped 2025](https://backstage.io/blog/2025/12/30/backstage-wrapped-2025/)
> - [Roadie - Backstage Ultimate Guide 2026](https://roadie.io/backstage-spotify/)

---

## 3. Coding Conventions (언어 무관 원칙)

### 3.1 핵심 원칙 (SOLID, DRY, KISS, YAGNI)

**CONSENSUS**

| 원칙 | 핵심 | 주의점 |
|------|------|--------|
| **SRP** (Single Responsibility) | 하나의 클래스/함수는 하나의 책임 | 과도하게 적용하면 클래스 폭발 |
| **OCP** (Open/Closed) | 확장에 열림, 수정에 닫힘 | 추상화 레이어가 과해질 수 있음 |
| **LSP** (Liskov Substitution) | 하위 타입은 상위 타입 대체 가능 | |
| **ISP** (Interface Segregation) | 클라이언트가 불필요한 인터페이스에 의존하지 않음 | |
| **DIP** (Dependency Inversion) | 고수준이 저수준에 의존하지 않음 | |
| **DRY** (Don't Repeat Yourself) | 로직 중복 제거 | "잘못된 추상화보다 중복이 낫다" (Sandi Metz) |
| **KISS** | 단순한 해결책 우선 | |
| **YAGNI** | 지금 필요 없는 것은 만들지 않음 | 미래 대비 과잉 설계 방지 |

**이 원칙들은 유연한 가이드라인이지 절대 규칙이 아님**. 프로젝트 맥락에 맞게 적응 필요.

---

### 3.2 네이밍 컨벤션

**CONSENSUS**

- **의도를 드러내는 이름**: `d` → `elapsedTimeInDays`
- **일관성**: 프로젝트 전체에서 하나의 네이밍 스키마 (camelCase / snake_case 등)
- **축약어 최소화**: `usr` → `user`, `btn` → `button` (팀 합의된 축약어는 예외)
- **불리언은 질문형**: `isActive`, `hasPermission`, `canDelete`
- **함수는 동사로 시작**: `getUserById`, `calculateTotal`, `validateInput`
- **상수는 대문자+언더스코어**: `MAX_RETRY_COUNT`

**DECISION NEEDED**

- 프로젝트별 네이밍 스타일 결정 (언어별 관례 따르기 vs 통일)
- 약어 허용 목록 정의

---

### 3.3 에러 핸들링 패턴

**CONSENSUS**

- **Fail Fast**: 잘못된 입력은 즉시 거부
- **에러 메시지는 명확하고 실행 가능**: 문제 + 원인 + 해결 방법 포함
- **에러 코드 체계**: 언어/로케일 무관한 숫자 코드 사용, 문서화
- **예외 vs 반환값**: 예외는 예외적 상황에만, 예상 가능한 실패는 반환값으로
- **에러 전파 시 컨텍스트 보존**: 원본 에러를 감싸되 스택 트레이스 유지
- **민감 데이터 노출 금지**: 에러 메시지에 DB 쿼리, 내부 경로 등 노출하지 않음

**DECISION NEEDED**

- 에러 코드 체계 설계 (범위별 구분 등)
- 에러 응답 표준 포맷 (RFC 7807 Problem Details 등)

---

### 3.4 로깅 표준

**CONSENSUS**

- **구조화된 로깅 (Structured Logging)**: JSON 포맷, 키-값 쌍으로 파싱 가능하게
- **필수 필드**: ISO 8601 타임스탬프(UTC), 로그 레벨, correlation ID, 서비스 이름
- **로그 레벨 올바르게 사용**:
  - `DEBUG`: 개발 시 디버깅 정보
  - `INFO`: 정상 동작의 의미 있는 이벤트
  - `WARN`: 잠재적 문제, 곧 에러가 될 수 있는 것
  - `ERROR`: 실패, 즉각 대응 필요
- **Correlation ID**: 분산 시스템에서 요청 체인 추적을 위해 고유 ID 전파
- **PII 마스킹**: 이메일, 신용카드, SSN 등 로깅 전 패턴 매칭으로 마스킹
- **로그하지 말 것**: 비밀번호, 토큰, 개인정보, 대량 데이터

**DECISION NEEDED**

- 로깅 프레임워크 선택 (언어별)
- 로그 집계 도구 (ELK, Loki, Datadog 등)
- 로그 보존 기간

> Sources:
> - [Better Stack - Logging Best Practices](https://betterstack.com/community/guides/logging/logging-best-practices/)
> - [Uptrace - Structured Logging](https://uptrace.dev/glossary/structured-logging)

---

### 3.5 코드 주석 - 언제 쓰고 언제 쓰지 않나

**CONSENSUS**

**작성해야 할 때 (WHY)**:
- 특정 구현 방식을 **선택한 이유**
- 복잡한 알고리즘의 **의도**
- 알려진 이슈, 잠재적 함정 (**TODO**, **FIXME**, **HACK**)
- 라이선스/법적 고지
- 공개 API 문서

**작성하지 말아야 할 때 (WHAT)**:
- 코드가 이미 명확히 말하는 것 (`i++; // increment i`)
- 모든 줄에 주석 (노이즈)
- 주석 처리된 코드 (Git을 사용하라)
- getter/setter 같은 자명한 함수

**핵심 원칙**: "좋은 코드는 대부분 스스로 설명한다. 주석은 코드가 말할 수 없는 것을 말한다."

> Sources:
> - [TechTarget - Code Comment Best Practices](https://www.techtarget.com/searchsoftwarequality/tip/Code-comment-best-practices-every-developer-should-know)
> - [Swimm - Comments in Code](https://swimm.io/learn/code-collaboration/comments-in-code-best-practices-and-mistakes-to-avoid)

---

## 4. Design & Architecture Best Practices

### 4.1 ADR (Architecture Decision Records)

**CONSENSUS**

**최소 구조** (Michael Nygard 템플릿):
```
# ADR-NNN: [제목]
## Status: [Proposed | Accepted | Deprecated | Superseded by ADR-XXX]
## Context: 어떤 상황에서 이 결정이 필요한가
## Decision: 무엇을 결정했는가
## Consequences: 이 결정의 긍정적/부정적 결과
```

**핵심 규칙**:
- **하나의 ADR = 하나의 결정**: 여러 결정을 합치지 않음
- **간결하게**: 1-2페이지, 지원 자료는 링크
- **대안 문서화**: 고려한 대안과 각각의 장단점 명시
- **적시 작성**: 컨텍스트가 생생할 때 작성, 나중에 쓰면 통찰을 잃음
- **Immutable**: 수정하지 않고, 새 ADR로 supersede
- **코드 리포에 저장**: `docs/adr/` 디렉토리, Markdown, 순번 부여 (001, 002...)
- **리뷰 방식**: Amazon 스타일 readout 미팅 (10-15분 묵독 → 서면 코멘트)
- **적정 수량**: 스타트업 2년 후 10-30개, 100+ 이상이면 과도

**DECISION NEEDED**

- ADR 템플릿 (Nygard 기본 vs 확장형)
- ADR 작성 기준 (어떤 결정이 "significant"인가)
- 리뷰/승인 프로세스

> Sources:
> - [AWS - Master ADR Best Practices](https://aws.amazon.com/blogs/architecture/master-architecture-decision-records-adrs-best-practices-for-effective-decision-making/)
> - [Martin Fowler - ADR](https://martinfowler.com/bliki/ArchitectureDecisionRecord.html)
> - [Spotify - When Should I Write an ADR](https://engineering.atspotify.com/2020/04/when-should-i-write-an-architecture-decision-record)

---

### 4.2 DESIGN.md 사용 가이드

**CONSENSUS**

- **목적**: 프로젝트/기능의 전체적인 설계 의도를 기술하는 living document
- **코드 리포에 위치**: `docs/design/` 또는 프로젝트 루트
- **ADR과 차이**: ADR = 개별 결정의 기록, DESIGN.md = 시스템 전체 설계의 현재 상태
- **포함 내용**: 시스템 개요, 핵심 컴포넌트, 데이터 흐름, 기술 스택 선택 근거, 제약 조건

**DECISION NEEDED**

- DESIGN.md 필수 여부 (프로젝트별 vs 전체)
- 업데이트 주기/책임자

---

### 4.3 C4 Model

**CONSENSUS**

| 레벨 | 대상 | 용도 | 대상 독자 |
|------|------|------|-----------|
| **L1: Context** | 시스템 + 외부 액터 | 전체 그림 | 모든 이해관계자 |
| **L2: Container** | 앱, DB, 메시지 큐 등 | 기술 스택 | 개발자, 아키텍트 |
| **L3: Component** | 컨테이너 내부 구성요소 | 상세 설계 | 개발팀 |
| **L4: Code** | 클래스/함수 수준 | (보통 불필요) | 특수한 경우만 |

**핵심 규칙**:
- **L1 + L2만으로 충분한 팀이 대부분** - 필요한 레벨만 사용
- **일관된 표기법**: 이름, 기호, 상세 수준 통일
- **범례 필수**: 색상, 형태, 약어, 선 스타일 모두 설명
- **모든 요소에 포함**: 이름, 요소 타입, 기술 선택, 설명 텍스트
- **정기 업데이트**: 시스템 변화 반영, 문서 부패 방지
- **도구**: Structurizr(Simon Brown 제작), PlantUML, Mermaid, draw.io

**DECISION NEEDED**

- C4 다이어그램 도구 선택
- 필수 레벨 범위 (L1+L2 vs L1+L2+L3)
- 다이어그램 저장 방식 (코드로 관리 vs 이미지)

> Sources:
> - [C4 Model 공식](https://c4model.com/)
> - [CloudAiry - C4 Diagrams Complete Guide 2026](https://cloudairy.com/blog/c4-diagrams-software-engineering)

---

### 4.4 Design Review Process

**CONSENSUS**

**리뷰 유형 2가지**:
- **Roadmap Review**: "이것을 해야 하는가?" (방향성)
- **Design Review**: "이 방법이 맞는가?" (구현)

**리뷰 패킷 필수 포함**:
- 문제 공간 요약
- 영향 받는 팀 목록
- 완료 시 비전
- 제안하는 아키텍처

**6개 차원 체크리스트**: 확장성, 보안, 유지보수성, 성능, 배포, 문서

**핵심 규칙**:
- **자기 설계를 자기가 리뷰하지 않음**: 도메인 전문가이면서 팀 외부인이 리뷰
- **체계적 리뷰로 프로덕션 인시던트 ~70% 예방**
- **Production Readiness Review**: 부하 테스트, 오토스케일링, 배포 자동화, 모니터링, 보안, DR 검증

**DECISION NEEDED**

- Design Review 필수 기준 (어떤 변경에 필수인가)
- Architecture Review Board 구성 여부 (소규모 팀에선 불필요할 수 있음)
- 리뷰 프로세스 (async 문서 리뷰 vs 동기 미팅)

> Sources:
> - [DevCom - Architecture Review Step-by-Step](https://devcom.com/tech-blog/successful-software-architecture-review-step-by-step-process/)
> - [AWS - Architecture Review Board](https://aws.amazon.com/blogs/architecture/build-and-operate-an-effective-architecture-review-board/)
> - [Red Hat - Design Review in Agile](https://www.redhat.com/en/blog/architecture-design-review)

---

### 4.5 API Design Principles

**CONSENSUS**

**공통 원칙 (REST / GraphQL 모두)**:
- **일관성**: 전체 API에서 동일한 네이밍, 페이지네이션, 에러 포맷
- **버전 관리**: URL path (`/v1/`, `/v2/`) 또는 헤더
- **페이지네이션**: 대규모 데이터셋에 필수 (cursor-based 권장)
- **Rate Limiting**: 역할 기반 요청 제한
- **에러 응답 표준화**: RFC 7807 Problem Details 형식 권장

**REST 원칙**:
- 리소스 중심 URL (`/users/{id}`)
- HTTP 메서드 의미 준수 (GET=조회, POST=생성, PUT=전체수정, PATCH=부분수정, DELETE=삭제)
- 적절한 HTTP 상태 코드 사용
- HATEOAS (Hypermedia) - 선택적

**GraphQL 원칙**:
- 클라이언트가 필요한 데이터만 요청 (over-fetching 방지)
- 스키마가 곧 문서
- N+1 문제 해결 (DataLoader 등)

**선택 기준 (2026 업계 트렌드)**:
- REST: 퍼블릭 API, 단순 CRUD, 캐싱 중요 (67% 팀 사용)
- GraphQL: 다양한 클라이언트 요구사항, 복잡한 데이터 관계 (40% 팀 파일럿)
- gRPC: 마이크로서비스 내부 통신, 저지연 (25% 팀 사용)

**DECISION NEEDED**

- API 스타일 선택 (REST vs GraphQL vs 하이브리드)
- API 버전 관리 전략
- API 문서화 도구 (OpenAPI/Swagger, GraphQL Playground 등)
- 인증 방식 (OAuth2, API Key, JWT)

> Sources:
> - [Xano - Modern API Design Best Practices 2026](https://www.xano.com/blog/modern-api-design-best-practices/)
> - [DEV - API Design Best Practices 2025](https://dev.to/cryptosandy/api-design-best-practices-in-2025-rest-graphql-and-grpc-2666)
> - [GraphQL Best Practices](https://graphql.org/learn/best-practices/)
