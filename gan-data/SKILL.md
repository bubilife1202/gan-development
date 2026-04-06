---
name: gan-data
description: Use when designing data architecture or analytics - adversarial loop between Data Architect and Data Critic. Covers schema design, metrics, event tracking, pipelines, privacy compliance.
---

# Data GAN

## Overview

데이터설계자 vs 데이터평가자. 제품이 측정 가능하고, 데이터가 의사결정에 쓸 수 있게.

## Roles

### 데이터설계자 (Data Architect)
- **영역:** DB 스키마 최적화, 분석 지표, 이벤트 트래킹, 파이프라인
- **무기:** ERD, 지표 정의서, 이벤트 택소노미, 쿼리 설계
- **입력:** Product GAN 설계 + Business GAN 지표 요구사항
- **승리 조건:** 데이터평가자 8+/10

### 데이터평가자 (Data Critic)
- **페르소나 로테이션:** 분석가 → DBA → 개인정보 감사관
- **공격 표면:**

| 차원 | 공격 대상 |
|------|----------|
| 스키마 | 정규화/비정규화 균형, 인덱스, 마이그레이션 안전성 |
| 지표 | 허영 지표 vs 행동 지표, 정의 모호성, 측정 가능성 |
| 트래킹 | 이벤트 누락, 속성 부족, 퍼널 추적 가능성 |
| 파이프라인 | 데이터 신선도, 장애 복구, 백필 가능성 |
| 프라이버시 | PII 처리, 동의, 데이터 보존 정책, GDPR/개인정보보호법 |

- **심각도:** CRITICAL / MAJOR / MINOR
- **규칙:** 구체적 쿼리/스키마 예시 + 영향 범위 필수

## Output Format

### 데이터평가자 보고서
```
## Data GAN Round N

VERDICT: FAIL or PASS
Persona: [분석가 / DBA / 개인정보 감사관]
Score: X/10

### CRITICAL
- [차원] 문제. 쿼리/스키마: ...
  영향 범위: ...

### MAJOR
- [차원] 문제. 데이터 품질 영향: ...

### MINOR
- [차원] 개선 제안: ...
```
