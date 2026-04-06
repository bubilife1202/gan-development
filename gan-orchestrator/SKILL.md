---
name: gan-orchestrator
description: Use when building or improving a product end-to-end - orchestrates 6 adversarial GAN loops in sequence (Business, Product, Content, Code+Data parallel, Growth). Each loop has a builder vs critic. Cross-role collaboration forbidden.
---

# GAN Orchestrator

## Overview

Orchestrates six independent adversarial loops. Each loop has a **builder** and a **critic** that fight until the critic scores 8+/10. Cross-role collaboration is **forbidden**.

## The 6 GAN Loops

```
Phase 1:  Business GAN    사업기획자 ⚔️ 사업평가자
Phase 2:  Product GAN     생성자 ⚔️ 제품평가자
Phase 3:  Content GAN     카피라이터 ⚔️ 콘텐츠평가자
Phase 4:  Code GAN ─┐     구현자 ⚔️ 코드평가자       ← 병렬
          Data GAN ─┘     데이터설계자 ⚔️ 데이터평가자
Phase 5:  Growth GAN      마케터 ⚔️ 성장평가자
```

## Isolation Rule

```
CROSS-ROLE COLLABORATION IS FORBIDDEN.
```

- 같은 GAN 루프 안에서만 싸운다 (builder ⚔️ critic)
- 다른 GAN의 builder/critic과 접촉 금지
- 유일한 연결: 이전 GAN의 **확정 결과물**이 다음 GAN의 **입력**으로 전달
- 전달 시 원본 그대로. 수정/해석 금지

**왜?** 다른 직무끼리 협력하면 관대해진다.

### 예외: 상위 GAN 결함 발견 시

하위 GAN에서 상위 설계의 결함을 발견하면:
1. 해당 GAN을 중단하고 결함을 보고
2. 해당 상위 GAN이 재개되어 결함 부분만 재싸움
3. 확정 후 하위 GAN 재개
4. 발견자가 직접 수정하지 않음. **보고만** 함

## Execution Instructions

1. **Phase 순서대로 실행** — 이전 Phase 확정 후 다음 Phase 시작
2. Phase 4a(Code)와 4b(Data)만 **병렬 실행**
3. 각 GAN 내에서 builder와 critic은 **별도 Agent로 실행**
4. 다른 GAN의 컨텍스트 전달 금지 (확정 결과물만 전달)
5. 각 GAN 최대 4라운드. 수렴 안 되면 orchestrator가 상위 3개 수정
6. 단일 GAN만 필요한 경우 → 해당 Phase만 실행 가능

### 라운드별 에스컬레이션 (모든 GAN 공통)
- **Round 1:** 표면적 문제 (명백한 결함, 누락)
- **Round 2:** 구조적 문제 (모순, 약한 논리, 확장성)
- **Round 3+:** 적대적 시나리오 (최악의 상황, 엣지케이스, 스트레스 테스트)

## Common Mistakes
- 다른 GAN의 builder/critic끼리 협력시킴 (관대해짐)
- Business GAN 스킵 (만들 가치가 없는 걸 열심히 만듦)
- Content GAN 스킵 (좋은 제품인데 카피가 구려서 전환 안 됨)
- Growth GAN 스킵 (좋은 제품인데 아무도 모름)
- 평가자가 너무 착함 (각 분야 최악의 비평가를 생각하라)
- builder가 다른 Phase의 결과를 멋대로 수정함 (보고만 해야 함)
