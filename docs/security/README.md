# Security

> Reference: [OWASP Top 10](https://owasp.org/www-project-top-ten/), [SLSA](https://slsa.dev/), [Snyk State of Open Source Security](https://snyk.io/reports/)

## OWASP Top 10 (2021/2025)

모든 프로젝트에서 인지하고 방어해야 할 10대 웹 보안 위험:

1. **Broken Access Control** — 접근 제어 실패
2. **Cryptographic Failures** — 암호화 실패
3. **Injection** — SQL, NoSQL, OS, LDAP 인젝션
4. **Insecure Design** — 설계 단계의 보안 결함
5. **Security Misconfiguration** — 보안 설정 오류
6. **Vulnerable Components** — 취약한/오래된 의존성
7. **Auth Failures** — 인증/식별 실패
8. **Data Integrity Failures** — 소프트웨어/데이터 무결성
9. **Logging/Monitoring Failures** — 보안 로깅 부재
10. **SSRF** — 서버측 요청 위조

## DevSecOps — Shift-Left

보안 검사를 파이프라인 초기 단계로 이동:

| 단계 | 도구 유형 | 시점 |
|------|----------|------|
| **Pre-commit** | Secret Detection | 커밋 전 |
| **PR** | SAST (정적 분석) | PR 생성 시 |
| **CI** | SCA (의존성 스캔) | 빌드 시 |
| **CI** | Container Scan | 이미지 빌드 후 |
| **Staging** | DAST (동적 분석) | 배포 후 |

## Secret Detection

시크릿이 Git에 커밋되는 것을 이중으로 방어:

1. **Pre-commit hook:** 커밋 전 로컬에서 차단
2. **CI:** Push 후 추가 스캔

프로젝트 환경에 따라 선택한다.

| 도구 | 적합한 상황 |
|------|------------|
| **gitleaks** | 기본 추천. 가장 활발한 커뮤니티, OSS |
| **truffleHog** | entropy 기반 탐지가 필요한 경우 |
| **git-secrets** | AWS 환경, 경량 도구 선호 시 |

## 의존성 스캔 (SCA)

소스 관리 플랫폼과 인프라 환경에 따라 선택한다.

| 도구 | 적합한 상황 |
|------|------------|
| **Dependabot** | GitHub 사용 시 기본. 무료, 자동 PR |
| **Trivy** | 컨테이너/IaC 스캔 필요 시. Dependabot과 병행 추천 |
| **Snyk** | 넓은 언어 지원 필요, 상용 예산 있을 시 |

## 정적 분석 (SAST)

소스 관리 플랫폼과 커스텀 룰 필요도에 따라 선택한다.

| 도구 | 적합한 상황 |
|------|------------|
| **CodeQL** | GitHub 사용 시 기본. 무료 |
| **Semgrep** | 커스텀 룰 유연, 빠름. OSS |
| **SonarQube** | 넓은 언어 커버리지, 상용 환경 |

## 공급망 보안

- **SBOM (Software Bill of Materials)** 생성 — CycloneDX 또는 SPDX 형식
- **SLSA 프레임워크** 준수 — 빌드 무결성 보장
- **의존성 Lock file 커밋** 필수
- **컨테이너 이미지 서명** — Sigstore/cosign 활용

## 기본 보안 체크리스트

- [ ] 인증/인가 설계 검토
- [ ] 입력 검증 (서버 사이드 필수)
- [ ] SQL/NoSQL 인젝션 방어 (파라미터 바인딩)
- [ ] XSS 방어 (출력 인코딩)
- [ ] CSRF 방어 (토큰 기반)
- [ ] HTTPS 강제
- [ ] 민감 데이터 암호화 (at rest + in transit)
- [ ] 에러 메시지에 내부 정보 노출 금지
- [ ] 보안 헤더 설정 (CSP, HSTS 등)
- [ ] Rate limiting 적용
