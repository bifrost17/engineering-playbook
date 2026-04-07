# AI-Assisted Development

> Reference: [Anthropic Best Practices](https://code.claude.com/docs/en/best-practices), [AGENTS.md](https://agents.md/), [GitHub Spec Kit](https://github.com/github/spec-kit), [Martin Fowler - Context Engineering](https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html)

## 컨텍스트 엔지니어링

2026년의 핵심 역량. "어떻게 물어볼까"가 아니라 **"AI에게 어떤 맥락을 어떤 구조로 제공할까"**.

### 5가지 원칙 (Addy Osmani / Spotify)

1. **선택(Selection)** — 관련 있는 컨텍스트만 제공
2. **압축(Compression)** — 핵심만 간결하게
3. **순서(Ordering)** — 중요한 것을 먼저
4. **격리(Isolation)** — 작업 단위를 작게 분리
5. **포맷 최적화** — 마크다운, 구조화된 형식

### ETH Zurich 경고 (2026.02)

| 시나리오 | 성공률 | 비용 |
|---------|--------|------|
| 컨텍스트 파일 없음 | 기준 | 기준 |
| LLM 자동 생성 | **-3%** | **+20%** |
| 사람이 작성 | +4% | +19% |

**교훈:** "덜 쓰되 정확하게." AI가 코드에서 추론 불가능한 정보만 기록.

## AI 에이전트 설정 파일

### 프로젝트에 필요한 파일

| 파일 | 필수 | 용도 |
|------|------|------|
| **AGENTS.md** | 권장 | 모든 AI 도구 호환, 범용 가이드 |
| **CLAUDE.md** | Claude 사용 시 | Claude Code 전용 추가 지시 |
| **DESIGN.md** | 프론트엔드 시 | UI 디자인 시스템 |

### CLAUDE.md 핵심 규칙

1. **60줄 이하** — 80줄 초과 시 무시 시작
2. **자동 생성 금지** — `/init` 사용하지 말 것
3. **추론 불가능한 정보만** — 빌드 명령, 비표준 설정, 고유 제약
4. **린터에 위임** — 코드 스타일은 CLAUDE.md에 넣지 말 것
5. **실수에서 진화** — AI가 틀릴 때마다 규칙 추가

## Spec-Driven Development

`/docs/documentation/` 참조. 요약:

```
Constitution → Spec → Plan → Tasks → Code
```

- AI에게 바로 코딩을 시키지 말고, **명세를 먼저 작성**
- "계획에 결함이 있으면 AI는 결함 있는 결과에 더 빨리 도달할 뿐"
- 한 번에 하나의 작업, 대규모 단일 요청 금지
- 빈번한 커밋과 체크포인트

## AI 코드 리뷰

- AI가 생성한 코드도 **반드시 사람이 리뷰**
- AI 리뷰 도구 (CodeRabbit 등)는 기계적 검사에만 활용
- **아키텍처, 비즈니스 로직, 보안은 사람이 판단**

## AI를 쓰지 말아야 할 때

- 보안 관련 코드 (인증, 암호화) — 사람이 직접 작성 + 전문가 리뷰
- 비즈니스 핵심 로직 — AI 생성 후 반드시 심층 리뷰
- 성능 크리티컬 코드 — 벤치마크 없이 AI 출력 신뢰 금지
- 규제 준수 코드 — 감사 추적 필요

## 토큰 세금 (Token Tax)

규칙 파일이 컨텍스트 윈도우의 최대 25%를 차지할 수 있다.

**완화 방법:**
- 모듈화 — 작업별 지침을 별도 파일로 분리
- 계층적 파일 — Global → Project → Local
- 작업 수준 프롬프팅 — 필요한 컨텍스트만 선택적 로드
