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
     Deadlock → STOP. 사용자에게 판단 요청. 자동 진행 금지. 미해결 항목을 제시하고 선택지 제공:
       a) 특정 항목 수용/기각 지정 후 진행 (강제 진행 시 잔여 결함은 리스크 레지스터에 추가)
       b) 추가 라운드 허용
       c) GAN 중단
```

## Handoff: Product GAN + Design GAN → Code GAN

Code GAN은 아래를 입력으로 받는다:
- Product GAN 확정 설계 패키지 전체 (유저 플로우, 페이지 목록, 정보 구조)
- Design GAN 확정 디자인 패키지 (디자인 토큰, 컴포넌트 명세, 접근성 체크리스트)
- Scorecard의 Code GAN 판정 기준 (Guardrail 지표 + 측정 방법)
- **리스크 레지스터** (Product + Design GAN에서 갱신된 최신 버전, 미완화 리스크 반드시 구현에서 고려)
- **결정 추적표** (CODE ← DSN ← PRD ← BIZ 매핑)

개발자는 기획/디자인 변경 권한 없음. 결함 발견 시 보고만 하고, 결함의 종류에 따라 해당 GAN이 재개되어 수정 후 복귀:
- 유저 플로우, 정보 구조, 기능 요구사항 관련 결함 → Product GAN 보고
- 디자인 토큰, 컴포넌트 명세, 시각 디자인 관련 결함 → Design GAN 보고
- 양쪽 모두 해당되는 경우 → Product GAN 보고 (Product가 Design에 재전파)

## Roles

### 개발자 (Developer)
- **영역:** 코드, 아키텍처, DB 스키마, API, 컴포넌트
- **무기:** 코드 작성, 리팩토링, 마이그레이션, 테스트
- **입력:** Product GAN 확정 설계 패키지 + Design GAN 확정 디자인 패키지
- **제약:** 기획/디자인 변경 권한 없음 (결함 발견 시 해당 GAN으로 보고만 가능)
- **제출 의무:**
  ```
  ## Builder Round N 제출

  ### 구현 내용
  - [CODE-001] 변경 파일:라인 + 변경 사유 (참조: PRD-00N, DSN-00N)

  ### 빌드/테스트 증빙
  - 빌드 명령: `<프로젝트 빌드 명령>` → 결과: PASS/FAIL (로그 첨부)
  - 테스트 명령: `<프로젝트 테스트 명령>` → 결과: N/M passed (로그 첨부)

  ### Guardrail 측정 증빙
  - [SC-C01] 측정 명령: `npx lighthouse URL --output=json` → 점수: N (목표: M)
  - [SC-C02] 측정 명령: `npm audit` → CRITICAL: N (목표: 0)

  ### 리스크 레지스터 대응
  - [RISK-001] → 코드에서 이렇게 완화: 파일:라인

  ### Critic 피드백 대응 (Round 2+)
  - [수용] 항목명 [CODE-00N] → 수정 파일:라인 + 변경 내용
  - [기각] 항목명 [CODE-00N] → 재현 불가 증빙 (테스트 코드/로그) 또는 설계상 불가 근거
  ```
- **결정ID 규칙:** `CODE-001` 형태. 상위 결정 참조 시 `(참조: PRD-00N, DSN-00N)`. 전체 추적 체인: CODE ← DSN ← PRD ← BIZ.
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

- **심각도:** CRITICAL / MAJOR / MINOR (MINOR는 채점에 영향 없음. 개선 제안용.)
- **규칙:**
  - 파일:라인 필수
  - **재현 절차 필수:** `1) X를 한다 2) Y를 한다 → 예상: Z, 실제: W`
  - 수정 제안 필수
  - Guardrail 지표 위반은 산식의 Guardrail FAIL 항으로만 반영 (CRITICAL 카운트에 미포함)
- **페르소나별 우선 공격 차원:**
  - Round 1 악의적 사용자: 보안 + 에러 차원 우선 공격. "이 입력으로 시스템을 깨뜨릴 수 있는가?"
  - Round 2 성능 감사관: 성능 + 확장성 차원 우선 공격. "1000배 트래픽에서 버티는가?"
  - Round 3+ 타입 시스템 집행자: 타입 + 정확성 차원 우선 공격. "런타임에 타입이 깨지는가?"
  - (모든 라운드에서 전 차원 공격 가능하나, 해당 페르소나의 우선 차원을 먼저 검토)

## Scoring Rubric

### 특수 규칙
- **빌드 실패:** 산식 적용 이전에 빌드 성공 여부를 확인. 빌드 실패 시 산식 결과와 무관하게 자동 FAIL (점수: 1). 빌드 성공 후 산식 적용.

산식: `기본 10점 - (미해결 CRITICAL × 4) - (미해결 MAJOR × 1.5) - (Guardrail FAIL × 2)`

| 산식 결과 | 판정 |
|----------|------|
| 8.0+     | PASS |
| <8.0     | FAIL |

미해결 CRITICAL이 1개라도 있으면 최대 6점이므로 자동 FAIL. Builder가 수정하면 Critic은 해당 CRITICAL을 철회하거나 MAJOR로 강등할 수 있다.

PASS = 8.0+.

### 해결/미해결 판정 기준
- **해결:** Builder가 수용하고 수정을 반영한 항목. 또는 Builder가 유효한 증거로 기각하여 Critic이 재채점 시 해당 항목을 철회한 경우.
- **미해결:** Builder가 수용했으나 수정이 미흡한 항목, 또는 증거 없이 기각하여 수용으로 간주된 항목, 또는 Critic이 재채점 시 여전히 유효하다고 판단한 항목.

## 확정 산출물 (Ship Package)

PASS 시 아래를 최종 산출물로 생성:

```
## Code GAN 확정 구현

### 1. 구현 요약
- [CODE-001] 파일 목록 + 변경 사유 (참조: PRD-00N, DSN-00N)

### 2. 빌드/테스트 증빙
- 빌드 명령: `...` → PASS
- 테스트 결과: N/M passed

### 3. Guardrail 달성 증빙
- [SC-C01] 측정 명령 + 결과 + 목표 → PASS
- [SC-C02] ...

### 4. 리스크 레지스터 (최종)
- [RISK-00N] 최종 상태 + 완화 방법 (코드 위치)

### 5. 결정 추적 (전체 체인)
- CODE-001 ← DSN-001 ← PRD-001 ← BIZ-001
```

## Output Format

### 코드평가자 보고서
```
## Code GAN Round N

VERDICT: FAIL or PASS
Persona: [악의적 사용자 / 성능 감사관 / 타입 시스템 집행자]
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
- [CODE-00N][FILE:LINE] 제안: ...

### Guardrail 검증
- [SC-C01] 명령: `...` → 결과: N (목표: M) → PASS/FAIL
```
