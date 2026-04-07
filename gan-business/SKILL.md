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
  1. Builder 제출 (초안 또는 수정본 + 증거 + 결정ID 부여)
  2. Critic 공격 (심각도별 분류 + 근거 + 결정ID 참조)
  3. Builder 반박/수용 선언 (항목별 수용/기각 + 이유)
  4. Critic 재채점 (rubric 산식 적용)
  5. PASS(8+) → 확정 산출물 생성 → 다음 GAN으로 핸드오프
     FAIL → Round N+1 (최대 4라운드)
     Deadlock(4라운드 후 미수렴) → STOP. 사용자에게 판단 요청.
       자동 진행 금지. 미해결 항목을 사용자에게 제시하고 선택지 제공:
       a) 특정 항목 수용/기각 지정 후 진행
       b) 추가 라운드 허용
       c) GAN 중단
```

## Roles

### 사업기획자 (Business Planner)
- **영역:** 시장 규모, PMF, 수익 모델, 가격 전략, 단위 경제
- **무기:** 비즈니스 캔버스, 수익 시뮬레이션, 경쟁 분석, 타겟 페르소나
- **제출 의무:** 매 라운드 아래 형식으로 제출
  ```
  ## Builder Round N 제출

  ### 제안/수정 내용
  - [BIZ-001] 결정 내용 + 근거
  - [BIZ-002] 결정 내용 + 근거

  ### 증거
  - [BIZ-001] [출처 유형: 벤치마크/사례/데이터] 내용 (URL 또는 출처 명시)

  ### Critic 피드백 대응 (Round 2+)
  - [수용] 항목명 [BIZ-00N] → 수정 내용
  - [기각] 항목명 [BIZ-00N] → 기각 사유 + 반박 증거
  ```
- **결정ID 규칙:** 핵심 결정에는 `BIZ-001`, 지표에는 `SC-P01`/`SC-D01`/`SC-C01`, 리스크에는 `RISK-001` 형태의 ID 부여. 이 ID가 하위 GAN까지 추적됨.
- **기각 규칙:** Critic 지적을 기각하려면 반드시 반박 증거를 제시해야 함. 증거 없는 기각은 수용으로 간주.
- **승리 조건:** 사업평가자 8+/10

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

- **심각도:** DEALBREAKER / MAJOR / CONCERN / NOTE (NOTE는 채점에 영향 없음. 참고사항 기록용.)
- **규칙:** 모든 지적에 반드시 출처/벤치마크/사례 근거 포함. 근거 없는 지적은 무효.
- **페르소나별 우선 공격 차원:**
  - Round 1 냉정한 VC: 시장 + PMF 차원 우선 공격. "이 시장이 존재하는가?"
  - Round 2 회의적인 CFO: 수익 + 경제 차원 우선 공격. "숫자가 맞는가?"
  - Round 3+ 공격적인 경쟁사 CEO: 경쟁 차원 우선 공격. "내가 이걸 카피하면?"
  - (모든 라운드에서 전 차원 공격 가능하나, 해당 페르소나의 우선 차원을 먼저 검토)

## Scoring Rubric

산식: `기본 10점 - (DEALBREAKER × 4) - (미해결 MAJOR × 2) - (미해결 CONCERN × 0.5)`

| 산식 결과 | 판정 |
|----------|------|
| 8.0+     | PASS |
| <8.0     | FAIL |

DEALBREAKER가 1개라도 있으면 최대 6점이므로 자동 FAIL.

PASS = 8.0+. 점수는 반드시 산식으로 계산하고, 각 항목 매핑을 명시.

### 해결/미해결 판정 기준
- **해결:** Builder가 수용하고 수정을 반영한 항목. 또는 Builder가 유효한 증거로 기각하여 Critic이 재채점 시 해당 항목을 철회한 경우.
- **미해결:** Builder가 수용했으나 수정이 미흡한 항목, 또는 증거 없이 기각하여 수용으로 간주된 항목, 또는 Critic이 재채점 시 여전히 유효하다고 판단한 항목.

## 확정 산출물 (Handoff Package)

PASS 시 아래를 반드시 포함하여 다음 GAN에 전달:

```
## Business GAN 확정 산출물

### 1. 사업모델 문서
- [BIZ-001] 핵심 결정 + 근거
- [BIZ-002] ...

### 2. 타겟 페르소나
- 주 페르소나: ...
- 부 페르소나: ...

### 3. Scorecard

#### 비즈니스 목표 지표
- [SC-BIZ01] 비즈니스 목표 지표명: 목표값 (근거: ...)
- [SC-BIZ02] ...

#### Product GAN 판정 기준 (Proxy 지표 — 세부 측정방법은 Product GAN이 정의)
- [SC-P01] 비즈니스 목표를 제품 지표로 번역: 목표값 (근거: ...)
- [SC-P02] ...

#### Design GAN 판정 기준 (Proxy 지표 — 세부 측정방법은 Design GAN이 정의)
- [SC-D01] 비즈니스 목표를 디자인 지표로 번역: 목표값 (근거: ...)

#### Code GAN 판정 기준 (Guardrail 지표 — 세부 측정 명령어/도구는 Code GAN이 정의)
- [SC-C01] 비즈니스 목표를 기술 지표로 번역: 목표값 (근거: ...)
- [SC-C02] ...

#### Launch 지표 (출시 후 검증 — 이 GAN에서는 판정에 사용 안 함)
- [SC-L01] 지표명: 목표값 (근거: ...) 검증 시점: 출시 후 N일

### 4. 리스크 레지스터
- [RISK-001] 리스크 내용 + 발생 조건 + 영향도 + 완화 방안
- (Deadlock으로 남은 항목이 있으면 여기에 포함)
```

## Output Format

### 사업평가자 보고서
```
## Business GAN Round N

VERDICT: FAIL or PASS
Persona: [VC / CFO / 경쟁사 CEO]
Score: X/10 (산식: 10 - DEALBREAKER×4 - MAJOR×2 - CONCERN×0.5 = X)

### DEALBREAKER
- [BIZ-00N][차원] 문제. 근거: [출처]
  투자 판단: ...

### MAJOR
- [BIZ-00N][차원] 문제. 근거: [출처]

### CONCERN
- [BIZ-00N][차원] 우려. 근거: [출처]

### NOTE
- [BIZ-00N][차원] 참고사항. 근거: [출처]
```
