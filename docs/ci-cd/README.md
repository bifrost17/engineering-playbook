# CI/CD

> Reference: [DORA State of DevOps](https://dora.dev), [CNCF GitOps](https://opengitops.dev/)

## 파이프라인 스테이지

```
Lint → Build → Unit Test → SAST/Secret Scan → Integration Test → Container Scan → Deploy(Staging) → E2E Test → Deploy(Prod)
```

- **Shift-left 원칙:** 보안/품질 검사를 파이프라인 초기로 이동
- **Pipeline-as-Code:** 파이프라인 자체도 코드로 관리, PR로 변경

## CI/CD 플랫폼

프로젝트의 소스 관리 플랫폼과 인프라 환경에 따라 선택한다.

| 플랫폼 | 적합한 상황 |
|--------|------------|
| **GitHub Actions** | GitHub 사용 시 자연스러운 선택. 시장점유율 ~50% |
| **GitLab CI** | 자체 호스팅 필요 시. 올인원 플랫폼 |
| **Jenkins** | 레거시 환경, 극도의 유연성 필요 시 |

## GitOps 원칙

CNCF 정의 (2024 graduated):

1. **선언적(Declarative)** 시스템 구성
2. **Git**을 Single Source of Truth로 사용
3. **자동 적용** (Automated Reconciliation)
4. **지속적 검증** (Continuous Verification)

Kubernetes 환경에서 프로젝트 규모에 따라 선택: ArgoCD (UI 풍부, 1위) vs FluxCD (경량).

## 환경 전략

최소 3-tier:

| 환경 | 용도 | 배포 |
|------|------|------|
| **dev** | 개발/실험 | PR 머지 시 자동 |
| **staging** | 프로덕션 미러 | main 머지 시 자동 |
| **prod** | 실서비스 | 수동 승인 또는 자동 |

- Staging은 prod와 동일 인프라 구성 (IaC로 보장)
- Feature environment (PR별 임시 환경) 활용 권장

## 배포 전략

서비스 규모와 다운타임 허용치에 따라 선택한다.

| 전략 | 적합한 상황 |
|------|------------|
| **Rolling Update** | 기본 선택. 간단하고 대부분의 서비스에 충분 |
| **Blue-Green** | 다운타임 제로 필요, 즉시 롤백 필요 |
| **Canary** | 대규모 서비스, 점진적 검증으로 위험 최소화 |

## 시크릿 관리

- **시크릿은 절대 Git에 커밋하지 않음** (Pre-commit hook으로 차단)
- 외부 시크릿 저장소 사용 필수
- CI/CD 변수는 마스킹 + 최소 권한 원칙

클라우드 종속성 허용 여부와 운영 역량에 따라 선택: Vault (범용) vs 클라우드 네이티브(AWS SM, GCP SM) vs SOPS+Age (경량).
