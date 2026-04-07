# GAN Development

Adversarial product development skills for Claude Code.

## Skills

| Skill | Builder | Critic |
|-------|---------|--------|
| `gan:business` | 사업기획자 | 사업평가자 |
| `gan:product` | 제품설계자 | 제품평가자 |
| `gan:design` | 디자이너 | 디자인평가자 |
| `gan:code` | 개발자 | 코드평가자 |

## Flow

```
Business GAN → (확정: 사업모델 + Scorecard + 리스크 레지스터)
  → Product GAN → (확정: 설계 패키지)
       ↓
  → Design GAN → (확정: 디자인 패키지)
       ↓              ↓
       └──────→ Code GAN → Ship it
```

Code GAN은 Product GAN과 Design GAN 양쪽의 확정 산출물을 입력으로 받는다.

## Core Rules

1. **GAN 루프 내 역할 분리.** 하나의 GAN 루프에서는 해당 GAN의 Builder와 Critic만 참여한다. 다른 GAN의 역할이 개입하거나, 루프 중 다른 GAN으로 전환할 수 없다. 단, 하위 GAN에서 상위 GAN의 결정에 결함 발견 시, 현재 루프를 중단하고 사용자에게 보고한 뒤 상위 GAN 재개 여부를 판단받는다.
2. **Round protocol is mandatory.** Builder 제출 → Critic 공격 → Builder 반박/수용 → Critic 재채점. 최대 4라운드.
3. **Deadlock = STOP.** 4라운드 미수렴 시 자동 진행 금지. 사용자에게 판단 요청.
4. **Evidence required.** Builder의 기각에는 반박 증거 필수. Critic의 공격에는 근거 필수. 증거 없는 주장은 무효.
5. **Scorecard flows down.** Business GAN이 확정한 지표가 하위 GAN의 판정 기준으로 전달. 하위 GAN은 매 라운드 측정값을 명시. 중간 GAN은 자신에게 해당하지 않는 하위 GAN용 Scorecard 지표를 수정할 수 없으며, 원본 그대로 pass-through한다. 지표 자체에 결함이 발견되면 상위 GAN 결함 보고 절차를 따른다.
6. **Scoring is formula-based.** 각 GAN은 고유 심각도 체계와 산식을 정의한다. 공통 원칙: `기본 10점 - (심각도별 가중치 합) - (Scorecard/Guardrail 감점)`. 8.0+ = PASS. 정확한 심각도 명칭, 가중치, Scorecard/Guardrail 감점 항목은 각 GAN의 Scoring Rubric 참조. 산식과 항목 매핑 필수.
7. **Traceability.** 모든 결정에 ID 부여 (BIZ-001 → PRD-001 → DSN-001 → CODE-001). 하위 결정은 상위를 참조.
8. **Risk register propagates.** 리스크 레지스터는 모든 핸드오프에 포함. 하위 GAN은 미완화 리스크를 반드시 고려.
9. **Handoff package is defined.** 각 GAN의 확정 산출물은 다음 GAN이 필요로 하는 항목을 빠짐없이 포함.

## Install

```bash
git clone https://github.com/bubilife1202/gan-development.git ~/.claude/skills/gan-development
```
