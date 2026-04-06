# GAN Development

Adversarial product development skills for Claude Code. Inspired by GANs — each skill runs a **builder vs critic** loop that iterates until the critic scores 8+/10.

## Skills

| Skill | Builder | Critic | Phase |
|-------|---------|--------|-------|
| `gan-orchestrator` | - | - | Full pipeline |
| `gan-business` | 사업기획자 | 사업평가자 | 1 |
| `gan-product` | 생성자 | 제품평가자 | 2 |
| `gan-content` | 카피라이터 | 콘텐츠평가자 | 3 |
| `gan-code` | 구현자 | 코드평가자 | 4a |
| `gan-data` | 데이터설계자 | 데이터평가자 | 4b |
| `gan-growth` | 마케터 | 성장평가자 | 5 |

## Flow

```
Business → Product → Content → Code + Data (parallel) → Growth → Ship it
```

## Core Rule

**Cross-role collaboration is forbidden.** Same GAN loop only. Different roles stay hostile.

## Install

```bash
# Clone into your Claude Code skills directory
git clone https://github.com/bubilife1202/gan-development.git ~/.claude/skills/gan-development
```

## Usage

```
/gan-orchestrator    # Run full pipeline
/gan-product         # Run product GAN only
/gan-code            # Run code GAN only
```
