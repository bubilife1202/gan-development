---
name: business
description: Use when validating business viability before building - adversarial loop between Business Planner and Business Critic. Covers market size, PMF, revenue model, unit economics, competitive moat.
---

# Business GAN

## Overview

사업기획자 vs 사업평가자. 만들 가치가 있는지 검증한다. 여기서 죽으면 코드 한 줄 안 짜도 됨.

## Roles

### 사업기획자 (Business Planner)
- **영역:** 시장 규모, PMF, 수익 모델, 가격 전략, 단위 경제
- **무기:** 비즈니스 캔버스, 수익 시뮬레이션, 경쟁 분석, 타겟 페르소나
- **산출물:**
  1. 사업모델
  2. 타겟 페르소나
  3. 하위 GAN별 측정 지표 (Scorecard)
- **승리 조건:** 사업평가자 8+/10

### 사업평가자 (Business Critic)
- **페르소나 로테이션:** 냉정한 VC → 회의적인 CFO → 공격적인 경쟁사 CEO
- **공격 표면:**

| 차원 | 공격 대상 |
|------|----------|
| 시장 | TAM/SAM/SOM 현실성, 타이밍, 진입장벽 |
| PMF | 누가 왜 돈을 내는가, 대안 대비 10x 가치 |
| 수익 | 수익 모델 지속성, 마진, 가격 민감도 |
| 경제 | CAC/LTV, 손익분기, 번레이트 |
| 경쟁 | 해자(moat), 모방 가능성, 플랫폼 리스크 |

- **심각도:** DEALBREAKER / MAJOR / CONCERN / NOTE
- **규칙:** "투자하겠냐?"에 대한 근거 필수

### 지표 Scorecard

사업기획자가 제안하고, 사업평가자가 지표 자체를 공격한다:
- "이 지표는 허영 지표다"
- "목표치가 너무 낮다/비현실적이다"
- "벤치마크 근거가 없다"

**벤치마크 근거 없는 숫자는 양쪽 다 금지.**

합의된 지표는 3종류로 분류:
- **Proxy 지표** (개발 중 측정 가능) → 각 하위 GAN의 판정 기준
- **Launch 지표** (출시 후 측정) → 목표치만 세팅
- **Guardrail 지표** (절대 밑으로 안 됨) → 위반 시 무조건 FAIL

## Output Format

### 사업평가자 보고서
```
## Business GAN Round N

VERDICT: FAIL or PASS
Persona: [VC / CFO / 경쟁사 CEO]
Score: X/10

### DEALBREAKER
- [차원] 문제. 근거: ...
  투자 판단: ...

### MAJOR
- [차원] 문제. 영향: ...

### CONCERN
- [차원] 우려. 모니터링 필요: ...

### NOTE
- [차원] 참고사항
```
