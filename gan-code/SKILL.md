---
name: code
description: Use when implementing or hardening code - adversarial loop between Implementer and Code Critic. Covers security, correctness, performance, type safety, error handling, scalability.
---

# Code GAN

## Overview

구현자 vs 코드평가자. 확정된 설계를 깨지지 않는 코드로 구현한다.

## Round Protocol

```
Round N:
  1. Builder 코드 제출 (구현 + 빌드 증빙 + Guardrail 지표 측정값)
  2. Critic 공격 (심각도별 + 파일:라인 + 재현 시나리오)
  3. Builder 반박/수용 (항목별 + 수정 커밋 또는 재현 불가 증빙)
  4. Critic 재채점 (rubric 기반)
  5. PASS(8+) → 확정
     FAIL → Round N+1 (최대 4라운드)
     Deadlock → 미해결 항목 + 리스크 태그 달고 진행
```

## Handoff: Product GAN → Code GAN

Code GAN은 아래를 입력으로 받는다:
- Product GAN 확정 설계
- **Scorecard의 Code GAN 판정 기준 (Guardrail 지표)**

구현자는 설계 변경 권한 없음. 설계 결함 발견 시 보고만 하고, Product GAN이 재개되어 수정 후 복귀.

## Roles

### 구현자 (Implementer)
- **영역:** 코드, 아키텍처, DB 스키마, API, 컴포넌트
- **무기:** 코드 작성, 리팩토링, 마이그레이션, 테스트
- **입력:** Product GAN 확정 설계
- **제약:** 설계 변경 권한 없음 (결함 보고만 가능)
- **제출 의무:**
  ```
  ## Builder Round N 제출

  ### 구현 내용
  - 변경 파일 목록 + 변경 사유

  ### 빌드/테스트 증빙
  - 빌드 성공 여부: PASS/FAIL
  - 테스트 결과: N/M passed

  ### Guardrail 지표 측정
  - Lighthouse 성능: N (목표: M)
  - CRITICAL 취약점: N (목표: 0)

  ### Critic 피드백 대응 (Round 2+)
  - [수용] 항목명 → 수정 파일:라인 + 변경 내용
  - [기각] 항목명 → 재현 불가 증빙 또는 반박 근거
  ```
- **기각 규칙:** Critic 지적을 기각하려면 재현 불가 증빙(테스트 코드, 로그) 또는 설계상 불가 근거 필수. 증거 없는 기각은 수용으로 간주.
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
  - **재현 시나리오 필수** (공격 스텝 1, 2, 3... → 예상 결과)
  - 수정 제안 필수
  - Guardrail 지표 위반 시 자동 CRITICAL

## Scoring Rubric

| 점수 | 기준 |
|------|------|
| 9-10 | CRITICAL 0, MAJOR 0, Guardrail 전부 달성, 빌드/테스트 PASS |
| 7-8 | CRITICAL 0, MAJOR ≤1 (수용됨), Guardrail 달성 |
| 5-6 | CRITICAL 0, MAJOR 2+ 미해결 |
| 3-4 | CRITICAL 1+ 미해결 또는 Guardrail 위반 |
| 1-2 | 빌드 실패 또는 핵심 기능 미작동 |

PASS = 8+.

## Output Format

### 코드평가자 보고서
```
## Code GAN Round N

VERDICT: FAIL or PASS
Score: X/10
Rubric 근거: CRITICAL N개, MAJOR N개, Guardrail 달성 N/M

### CRITICAL
- [FILE:LINE] 문제.
  재현: 1) ... 2) ... 3) ... → 결과: ...
  수정 제안: ...

### MAJOR
- [FILE:LINE] 문제. 영향: ...
  수정: ...

### MINOR
- [FILE:LINE] 제안: ...

### Guardrail 검증
- [지표명] 목표: N, 현재: M → PASS/FAIL
```
