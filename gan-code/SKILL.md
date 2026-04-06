---
name: code
description: Use when implementing or hardening code - adversarial loop between Implementer and Code Critic. Covers security, correctness, performance, type safety, error handling, scalability.
---

# Code GAN

## Overview

구현자 vs 코드평가자. 확정된 설계+카피를 깨지지 않는 코드로 구현한다.

## Roles

### 구현자 (Implementer)
- **영역:** 코드, 아키텍처, DB 스키마, API, 컴포넌트
- **무기:** 코드 작성, 리팩토링, 마이그레이션, 테스트
- **입력:** Product GAN 설계 + Content GAN 카피
- **제약:** 설계/카피 변경 권한 없음 (결함 보고만 가능)
- **승리 조건:** 코드평가자 8+/10

### 코드평가자 (Code Critic)
- **페르소나 로테이션:** 악의적 사용자 → 성능 감사관 → 타입 시스템 집행자
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
- **규칙:** 파일:라인 + 공격 시나리오 + 수정 제안 필수

## Output Format

### 코드평가자 보고서
```
## Code GAN Round N

VERDICT: FAIL or PASS
Score: X/10

### CRITICAL
- [FILE:LINE] 문제. 공격 시나리오: ...
  수정 제안: ...

### MAJOR
- [FILE:LINE] 문제. 영향: ...
  수정: ...

### MINOR
- [FILE:LINE] 제안: ...
```
