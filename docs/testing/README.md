# Testing

> Reference: [Martin Fowler - TestPyramid](https://martinfowler.com/bliki/TestPyramid.html), [Kent C. Dodds - Testing Trophy](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications)

## 핵심 원칙

- **모든 PR에 관련 테스트 포함** — 코드 변경 = 테스트 변경
- **CI에서 자동 실행**, 실패 시 머지 불가
- **Flaky test는 즉시 격리 및 수정**

## 커버리지

- **새 코드:** 80-90%
- **전체 프로젝트:** 70-80% 유지
- 커버리지 수치보다 **중요 경로(critical path) 커버리지**가 더 중요
- CI에서 커버리지 게이트 설정

## 테스트 유형

| 유형 | 범위 | 속도 | 신뢰도 |
|------|------|------|--------|
| **Unit** | 단일 함수/클래스 | 빠름 | 낮음 |
| **Integration** | 컴포넌트 간 상호작용 | 중간 | 높음 |
| **E2E** | 전체 사용자 시나리오 | 느림 | 가장 높음 |

## 테스트 비율 모델

프로젝트 특성에 따라 선택한다. 결정 시 ADR로 기록할 것.

| 모델 | 적합한 상황 |
|------|------------|
| **Test Pyramid** (Unit 중심) | 라이브러리, 백엔드 API, 알고리즘 |
| **Test Trophy** (Integration 중심) | 프론트엔드 앱, React/Vue |
| **Test Diamond** (Integration 최다) | 마이크로서비스, API 중심 |

2024-2026 트렌드: Integration 테스트 비중 확대가 공통.

## 테스트 작성 시점

팀 역량과 프로젝트 성격에 따라 선택한다.

- **TDD:** 설계 품질 향상, 진입장벽 높음 (~15-20% 채택)
- **Write-Tests-After:** 현실적, 진입장벽 낮음 (~60-70% 채택)
- **실용적 접근:** "PR에 테스트 포함 필수" 규칙이 TDD 강제보다 실효성 높음

## 테스트 작성 가이드

### 구조: Arrange-Act-Assert (AAA)

```
// Arrange: 테스트 데이터 준비
// Act: 테스트 대상 실행
// Assert: 결과 검증
```

### 네이밍

`should_[expected]_when_[condition]`

예: `should_return_404_when_user_not_found`

### Don't

- 구현 세부사항 테스트 (리팩토링 시 깨짐)
- Sleep/시간 의존 테스트 (Flaky)
- 외부 서비스 직접 호출 (Mock/Stub 사용)
- 하나의 테스트에 여러 시나리오
