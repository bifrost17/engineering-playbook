# Design & Architecture

> Reference: [MADR](https://adr.github.io/madr/), [C4 Model](https://c4model.com/), [DESIGN.md](https://github.com/VoltAgent/awesome-design-md)

## Architecture Decision Records (ADR)

모든 주요 기술 결정은 ADR로 기록한다.

> AI가 코드에서 What/How는 추론할 수 있지만, **Why는 추론할 수 없다.** 이것이 ADR의 핵심 가치.

### 규칙

- **하나의 ADR = 하나의 결정** — 여러 결정을 섞지 않음
- **결정 전 작성** — 사후 기록이 아닌, 논의 도구로 활용
- **짧게** — 1-2페이지 이내
- **Superseded 표시** — 결정이 바뀌면 기존 ADR에 표시하고 새 ADR 작성
- **저장 위치:** `docs/adr/0001-title.md` (번호순)
- **템플릿:** `/templates/adr-template.md` 사용

### 언제 ADR을 작성하는가

- 기술 스택 선택 (언어, 프레임워크, DB)
- 아키텍처 패턴 변경 (모놀리스 → 마이크로서비스 등)
- 외부 서비스/라이브러리 도입
- 보안/인증 방식 결정
- 되돌리기 어려운 모든 결정

## DESIGN.md

프론트엔드 프로젝트에서 AI 에이전트가 일관된 디자인을 생성하도록 하는 디자인 시스템 문서.

- **AGENTS.md** = "어떻게 빌드"
- **DESIGN.md** = "어떻게 보여야 하는지"
- **템플릿:** `/templates/DESIGN.md` 사용

## 아키텍처 다이어그램 — C4 Model

> Reference: [Simon Brown - C4 Model](https://c4model.com/)

대부분의 프로젝트에서 **Level 1 + Level 2만으로 충분**.

| Level | 이름 | 대상 | 설명 |
|-------|------|------|------|
| **L1** | System Context | 비개발자 포함 | 시스템과 외부 시스템/사용자 관계 |
| **L2** | Container | 개발팀 | 서비스, DB, 메시지 큐 등 주요 컨테이너 |
| L3 | Component | 필요 시 | 컨테이너 내부 컴포넌트 (선택) |
| L4 | Code | 거의 불필요 | 클래스 다이어그램 (IDE가 대체) |

**도구:** Structurizr DSL, Mermaid, PlantUML (코드로 다이어그램 → Git 추적 가능)

## 설계 리뷰

### 언제 설계 리뷰를 하는가

- 새로운 서비스/시스템 도입
- 기존 아키텍처의 큰 변경
- 외부 의존성 추가
- 보안에 영향을 미치는 변경

### 프로세스

1. RFC 또는 Design Doc 작성 (`/templates/rfc-template.md`)
2. 팀에 공유, 비동기 피드백 수집 (최소 2일)
3. 필요 시 동기 회의로 논의
4. 결정 사항을 ADR로 기록

## API 설계

클라이언트 유형과 서비스 특성에 따라 선택한다. 혼합 사용도 일반적.

| 방식 | 적합한 상황 |
|------|------------|
| **REST** | CRUD 중심, 단순한 클라이언트, 기본 선택 (~67% 채택) |
| **GraphQL** | 다양한 클라이언트, 유연한 쿼리 필요 (~40% 채택) |
| **gRPC** | 서비스 간 통신, 고성능 필요 (~25% 채택) |

공통 원칙 (방식 무관):
- API 버저닝 필수 (`/v1/`, `/v2/`)
- 일관된 에러 응답 형식
- 인증/인가 설계 선행
- API 문서 자동 생성 (OpenAPI/Swagger)
