# Observability

> Reference: [OpenTelemetry](https://opentelemetry.io/), [Google SRE Book](https://sre.google/sre-book/table-of-contents/), [CNCF Observability Whitepaper](https://www.cncf.io/)

## 3대 축 (Three Pillars)

| 축 | 목적 | 핵심 |
|----|------|------|
| **Logs** | 이벤트 기록 | 구조화된 JSON, correlation ID |
| **Metrics** | 시계열 수치 | RED (Rate, Errors, Duration) |
| **Traces** | 분산 추적 | 서비스 간 요청 흐름 |

## OpenTelemetry (OTel) — 업계 표준

> CNCF Graduated (2024). 벤더 중립 계측 표준.

- 애플리케이션은 **OTel SDK로 계측** → 백엔드는 나중에 교체 가능 (vendor lock-in 방지)
- Logs, Metrics, Traces 통합
- 대부분의 벤더 (Datadog, Grafana, New Relic, Dynatrace)가 OTel 수집 지원

## 관측 스택

> **[DECISION NEEDED]**

| 스택 | 구성 | 특징 |
|------|------|------|
| **Grafana Stack** | Loki + Mimir + Tempo | OSS, 비용 효율적, 2024-2025 성장세 1위 |
| **ELK** | Elasticsearch + Logstash + Kibana | 성숙, 강력한 검색 |
| **Datadog** | SaaS 올인원 | 운영 부담 최소, 비용 높음 |
| **Self-hosted vs SaaS** | | 팀 규모와 운영 역량에 따라 |

## 구조화된 로깅

`/docs/coding-conventions/` 로깅 섹션 참조. 핵심 재강조:

- JSON 형식 필수
- 모든 로그에 `traceId` 포함
- 프로덕션은 INFO 이상
- 민감 정보 로깅 금지

## SLO / SLI / SLA

> Reference: [Google SRE - SLOs](https://sre.google/sre-book/service-level-objectives/)

| 용어 | 정의 | 예시 |
|------|------|------|
| **SLI** | 측정 가능한 지표 | 응답 시간 p99, 에러율 |
| **SLO** | 내부 목표 | 가용성 99.9%, p99 < 200ms |
| **SLA** | 고객 계약 | SLO보다 여유 있게 (99.9% SLO → 99.5% SLA) |

**Error Budget:** SLO 기준 허용 오류량. 소진 시 새 기능 개발 중단하고 안정성에 집중.

## 알림 (Alerting)

### 원칙

- **행동 가능한(actionable) 알림만** 발송
- **증상(symptom) 기반** 알림 — 원인(cause) 기반은 대시보드로
- 30일간 action 없는 알림은 제거/조정

### 계층화

| 등급 | 대응 | 알림 방식 |
|------|------|----------|
| P1 | 즉시 대응 | PagerDuty/전화 |
| P2 | 업무시간 내 | Slack |
| P3 | 백로그 | 이메일/티켓 |
