# GAN Development

Adversarial product development skills for Claude Code.

## Skills

| Skill | Builder | Critic |
|-------|---------|--------|
| `gan:business` | 사업기획자 | 사업평가자 |
| `gan:product` | 생성자 | 제품평가자 |
| `gan:code` | 구현자 | 코드평가자 |

## Flow

```
Business GAN → (확정: 사업모델 + Scorecard) → Product GAN → (확정: 설계) → Code GAN → Ship it
```

## Core Rules

1. **Cross-role collaboration is forbidden.** Same GAN loop only.
2. **Round protocol is mandatory.** Builder 제출 → Critic 공격 → Builder 반박/수용 → Critic 재채점. 최대 4라운드.
3. **Evidence required.** Builder의 기각에는 반박 증거 필수. Critic의 공격에는 근거/재현 시나리오 필수. 증거 없는 주장은 무효.
4. **Scorecard flows down.** Business GAN이 확정한 지표가 Product/Code GAN의 판정 기준으로 전달된다. 하위 GAN은 이 지표를 매 라운드 측정해야 한다.
5. **Scoring is rubric-based.** 8+/10 = PASS. 점수는 반드시 rubric 표에 매핑해서 산출.

## Install

```bash
git clone https://github.com/bubilife1202/gan-development.git ~/.claude/skills/gan-development
```
