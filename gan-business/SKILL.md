---
name: business
description: Use when validating business viability before building - adversarial loop between Business Planner and Business Critic. Covers market size, PMF, revenue model, unit economics, competitive moat.
---

# Business GAN

## Overview

사업기획자 vs 사업평가자. 만들 가치가 있는지 검증한다. 여기서 죽으면 코드 한 줄 안 짜도 됨.

## Round Protocol

```
Round N:
  1. Builder 제출 (초안 또는 수정본 + 증거)
  2. Critic 공격 (심각도별 분류 + 근거)
  3. Builder 반박/수용 선언 (항목별 수용/기각 + 이유)
  4. Critic 재채점 (rubric 기반)
  5. PASS(8+) → 확정 산출물 생성 → 다음 GAN으로 핸드오프
     FAIL → Round N+1 (최대 4라운드)
     Deadlock(4라운드 후 미수렴) → 미해결 항목 목록 + 리스크 태그 달고 진행
```

## Roles

### 사업기획자 (Business Planner)
- **영역:** 시장 규모, PMF, 수익 모델, 가격 전략, 단위 경제
- **무기:** 비즈니스 캔버스, 수익 시뮬레이션, 경쟁 분석, 타겟 페르소나
- **제출 의무:** 매 라운드 아래 형식으로 제출
  ```
  ## Builder Round N 제출

  ### 제안/수정 내용
  (변경사항 + 근거)

  ### 증거
  - [출처 유형: 벤치마크/사례/데이터] 내용 (URL 또는 출처 명시)

  ### Critic 피드백 대응 (Round 2+)
  - [수용] 항목명 → 수정 내용
  - [기각] 항목명 → 기각 사유 + 반박 증거
  ```
- **기각 규칙:** Critic 지적을 기각하려면 반드시 반박 증거를 제시해야 함. 증거 없는 기각은 수용으로 간주.
- **산출물 (확정 시):**
  1. 사업모델 문서
  2. 타겟 페르소나
  3. **하위 GAN Scorecard** (아래 형식)

### 사업평가자 (Business Critic)
- **페르소나 로테이션:** Round 1 냉정한 VC → Round 2 회의적인 CFO → Round 3+ 공격적인 경쟁사 CEO
- **공격 표면:**

| 차원 | 공격 대상 |
|------|----------|
| 시장 | TAM/SAM/SOM 현실성, 타이밍, 진입장벽 |
| PMF | 누가 왜 돈을 내는가, 대안 대비 10x 가치 |
| 수익 | 수익 모델 지속성, 마진, 가격 민감도 |
| 경제 | CAC/LTV, 손익분기, 번레이트 |
| 경쟁 | 해자(moat), 모방 가능성, 플랫폼 리스크 |

- **심각도:** DEALBREAKER / MAJOR / CONCERN / NOTE
- **규칙:** "투자하겠냐?"에 대한 근거 필수. 모든 지적에 반드시 출처/벤치마크/사례 근거 포함.

## Scoring Rubric

| 점수 | 기준 |
|------|------|
| 9-10 | DEALBREAKER 0, MAJOR 0, 모든 지표에 벤치마크 근거 있음 |
| 7-8 | DEALBREAKER 0, MAJOR ≤1 (수용됨), 핵심 지표에 근거 있음 |
| 5-6 | DEALBREAKER 0, MAJOR 2+ 미해결 |
| 3-4 | DEALBREAKER 1+ 미해결 |
| 1-2 | 사업모델 자체가 성립 안 함 |

PASS = 8+. 점수는 반드시 rubric 기준으로 산출하고, 각 항목 매핑을 명시.

## Scorecard (확정 산출물)

사업기획자가 제안하고, 사업평가자가 지표 자체를 공격한다:
- "이 지표는 허영 지표다"
- "목표치가 너무 낮다/비현실적이다"
- "벤치마크 근거가 없다"

**벤치마크 근거 없는 숫자는 양쪽 다 금지.**

합의된 지표는 3종류로 분류하여 하위 GAN에 전달:

```
## Scorecard (Business GAN 확정)

### Product GAN 판정 기준 (Proxy 지표)
- 핵심 전환까지 클릭 수: ≤N (근거: ...)
- 온보딩 완료율 목표: N% (근거: ...)

### Code GAN 판정 기준 (Guardrail 지표)
- Lighthouse 성능: ≥N (근거: ...)
- CRITICAL 취약점: 0

### Launch 지표 (출시 후 검증)
- D7 리텐션: N% (근거: 유사 서비스 벤치마크)
- 월간 전환율: N% (근거: ...)
```

## Output Format

### 사업평가자 보고서
```
## Business GAN Round N

VERDICT: FAIL or PASS
Persona: [VC / CFO / 경쟁사 CEO]
Score: X/10
Rubric 근거: DEALBREAKER N개, MAJOR N개 (미해결 N개)

### DEALBREAKER
- [차원] 문제. 근거: [출처]
  투자 판단: ...

### MAJOR
- [차원] 문제. 근거: [출처]
  영향: ...

### CONCERN
- [차원] 우려. 모니터링 필요: ...

### NOTE
- [차원] 참고사항
```
