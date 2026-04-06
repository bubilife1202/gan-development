---
name: code
description: Use when implementing or hardening code - adversarial loop between Developer and Code Critic. Covers security, correctness, performance, type safety, error handling, scalability.
---

# Code GAN

## Overview

개발자 vs 코드평가자. 확정된 설계를 깨지지 않는 코드로 구현한다.

## Round Protocol

```
Round N:
  1. Builder 코드 제출 (구현 + 빌드 증빙 + Guardrail 측정 로그)
  2. Critic 공격 (심각도별 + 파일:라인 + 재현 절차)
  3. Builder 반박/수용 (항목별 + 수정 커밋 또는 재현 불가 증빙)
  4. Critic 재채점 (rubric 산식 적용)
  5. PASS(8+) → 확정
     FAIL → Round N+1 (최대 4라운드)
     Deadlock → STOP. 사용자에게 판단 요청. 자동 진행 금지.
```

## Handoff: Product GAN → Code GAN

Code GAN은 아래를 입력으로 받는다:
- Product GAN 확정 설계 패키지 전체 (유저 플로우, 페이지 목록, 정보 구조)
- Scorecard의 Code GAN 판정 기준 (Guardrail 지표 + 측정 방법)
- **리스크 레지스터** (미완화 리스크 반드시 구현에서 고려)
- **Business 결정 추적표** (PRD ← BIZ 매핑)

개발자는 설계 변경 권한 없음. 설계 결함 발견 시 보고만 하고, Product GAN이 재개되어 수정 후 복귀.

## Roles

### 개발자 (Developer)
- **영역:** 코드, 아키텍처, DB 스키마, API, 컴포넌트
- **무기:** 코드 작성, 리팩토링, 마이그레이션, 테스트
- **입력:** Product GAN 확정 설계 패키지
- **제약:** 설계 변경 권한 없음 (결함 보고만 가능)
- **제출 의무:**
  ```
  ## Builder Round N 제출

  ### 구현 내용
  - [CODE-001] 변경 파일:라인 + 변경 사유 (참조: PRD-00N)

  ### 빌드/테스트 증빙
  - 빌드 명령: `npm run build` → 결과: PASS/FAIL (로그 첨부)
  - 테스트 명령: `npm test` → 결과: N/M passed (로그 첨부)

  ### Guardrail 측정 증빙
  - [SC-C01] 측정 명령: `npx lighthouse URL --output=json` → 점수: N (목표: M)
  - [SC-C02] 측정 명령: `npm audit` → CRITICAL: N (목표: 0)

  ### 리스크 레지스터 대응
  - [RISK-001] → 코드에서 이렇게 완화: 파일:라인

  ### Critic 피드백 대응 (Round 2+)
  - [수용] 항목명 → 수정 파일:라인 + 변경 내용
  - [기각] 항목명 → 재현 불가 증빙 (테스트 코드/로그) 또는 설계상 불가 근거
  ```
- **결정ID 규칙:** `CODE-001` 형태. Product 결정 참조 시 `(참조: PRD-00N)`.
- **기각 규칙:** 재현 불가 증빙(테스트 코드, 실행 로그) 또는 설계상 불가 근거 필수. 증거 없는 기각은 수용으로 간주.
- **승리 조건:** 코드평가자 8+/10

### 코드평가자 (Code Critic)
- **페르소나 로테이션:** Round 1 악의적 사용자 → Round 2 성능 감사관 → Round 3+ 타입 시스템 집행자
- **공격 표면:**

| 차원 | 공격 대상 |
|------|----------|
| 보안 | 인증 우회, 인젝션, XSS, CSRF, RLS, 권한 상승 |
| 정확성 | 레이스 컨디션, 엣지케이스, 데이터 무결성, 트랜잭션 |
| 성능 | N+1 쿼리, 페이지네이션, 번들 사이즈, 렌더 사이클 |
| 타입 | 안전하지 않은 캐스팅, 검증 누락, 스키마 불일치 |
| 에러 | 미처리 실패, 조용한 에러, 사용자 피드백 부재 |
| 확장성 | DB 인덱스, 쿼리 패턴, 캐싱, 커넥션 풀링 |

- **심각도:** CRITICAL / MAJOR / MINOR
- **규칙:**
  - 파일:라인 필수
  - **재현 절차 필수:** `1) X를 한다 2) Y를 한다 → 예상: Z, 실제: W`
  - 수정 제안 필수
  - Guardrail 지표 위반 시 자동 CRITICAL

## Scoring Rubric

산식: `기본 10점 - (CRITICAL × 4) - (미해결 MAJOR × 1.5) - (Guardrail FAIL × 2)`

| 점수 | 산식 결과 |
|------|----------|
| 9-10 | 9.0+ |
| 8 | 8.0-8.9 |
| 7 | 7.0-7.9 |
| 5-6 | 5.0-6.9 |
| 3-4 | 3.0-4.9 |
| 1-2 | <3.0 또는 빌드 실패 |

PASS = 8.0+.

## Output Format

### 코드평가자 보고서
```
## Code GAN Round N

VERDICT: FAIL or PASS
Score: X/10 (산식: 10 - CRIT×4 - MAJOR×1.5 - GR_FAIL×2 = X)

### CRITICAL
- [CODE-00N][FILE:LINE] 문제.
  재현: 1) ... 2) ... → 예상: ... 실제: ...
  수정 제안: ...

### MAJOR
- [CODE-00N][FILE:LINE] 문제.
  재현: 1) ... 2) ... → 예상: ... 실제: ...
  수정: ...

### MINOR
- [FILE:LINE] 제안: ...

### Guardrail 검증
- [SC-C01] 명령: `...` → 결과: N (목표: M) → PASS/FAIL
```
