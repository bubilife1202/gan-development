---
name: gan-growth
description: Use when planning growth strategy - adversarial loop between Growth Marketer and Growth Critic. Covers acquisition channels, viral loops, retention, referral, activation, monetization.
---

# Growth GAN

## Overview

마케터 vs 성장평가자. 만들어진 제품을 어떻게 퍼뜨리고 유지할 것인가.

## Roles

### 마케터 (Growth Marketer)
- **영역:** 획득 채널, 바이럴 루프, 리텐션 메커니즘, 레퍼럴
- **무기:** 성장 실험 설계, 채널 전략, 퍼널 최적화, 리텐션 곡선
- **입력:** 완성된 제품 + Business GAN의 타겟/포지셔닝
- **승리 조건:** 성장평가자 8+/10

### 성장평가자 (Growth Critic)
- **페르소나 로테이션:** 냉소적인 그로스 해커 → 예산 0원 스타트업 대표 → 이탈한 사용자
- **공격 표면:**

| 차원 | 공격 대상 |
|------|----------|
| 획득 | 채널 의존도, CAC 현실성, 오가닉 vs 유료 비율 |
| 활성화 | 아하 모먼트까지의 시간, 첫 가치 경험 |
| 리텐션 | D1/D7/D30, 이탈 원인, 재참여 메커니즘 |
| 바이럴 | K-factor, 공유 동기, 네트워크 효과 유무 |
| 수익화 | 전환율, ARPU, 업셀 경로, 가격 실험 |
| 레퍼럴 | 추천 인센티브, 양면 보상, 자연 추천 vs 인위적 |

- **심각도:** DEALBREAKER / MAJOR / WEAK / IDEA
- **규칙:** 숫자 근거 필수 (벤치마크, 업계 평균, 예상 전환율)

## Output Format

### 성장평가자 보고서
```
## Growth GAN Round N

VERDICT: FAIL or PASS
Persona: [그로스 해커 / 예산 0원 대표 / 이탈 사용자]
Score: X/10

### DEALBREAKER
- [차원] 문제. 벤치마크: ...
  현실적 예측: ...

### MAJOR
- [차원] 문제. 성장 병목: ...

### WEAK
- [차원] 약한 부분. 개선 방향: ...

### IDEA
- [차원] 실험 제안: ...
```
