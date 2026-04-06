---
name: gan-product
description: Use when designing or improving product UX - adversarial loop between Designer and Product Critic. Covers UX/UI, usability, user flows, onboarding, empty states, accessibility, mobile responsiveness.
---

# Product GAN

## Overview

생성자 vs 제품평가자. 확정된 사업모델을 제품으로 설계한다.

## Roles

### 생성자 (Designer)
- **영역:** UX, 플로우, 정보구조, 와이어프레임, 인터랙션
- **무기:** 플로우차트, 와이어프레임, 유저저니맵
- **입력:** Business GAN의 확정된 사업모델/페르소나
- **승리 조건:** 제품평가자 8+/10

### 제품평가자 (Product Critic)
- **페르소나 로테이션:** 첫 방문 사용자 → 파워 유저 → 접근성 감사관
- **공격 표면:**

| 차원 | 공격 대상 |
|------|----------|
| UX/UI | 네비게이션, 인지 부하, 시각적 위계, 반응형 |
| 사용성 | 온보딩 마찰, 에러 복구, 접근성, 빈/로딩 상태 |
| 플로우 | 핵심 전환 경로, 이탈 지점, 데드엔드 |
| 심리 | 동기 부여, 습관 형성, 보상 타이밍, 사회적 증거 |
| 일관성 | 패턴 통일, 예측 가능성, 학습 곡선 |

- **심각도:** DEALBREAKER / MAJOR / FRICTION / POLISH
- **규칙:** 유저 시나리오 + "잘하면 이렇다" 필수

## Output Format

### 제품평가자 보고서
```
## Product GAN Round N

VERDICT: FAIL or PASS
Persona: [첫 방문 사용자 / 파워 유저 / 접근성 감사관]
Score: X/10

### DEALBREAKER
- [차원] 문제. 유저 시나리오: "나는..."
  잘하면 이렇다: ...

### MAJOR
- [차원] 문제. 리텐션/전환 영향: ...

### FRICTION
- [차원] 불편한 순간: ...

### POLISH
- [차원] 있으면 좋은 것
```
