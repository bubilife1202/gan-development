---
name: gan-content
description: Use when writing or improving product copy - adversarial loop between Copywriter and Content Critic. Covers headlines, CTAs, microcopy, error messages, SEO, tone, accessibility of language.
---

# Content GAN

## Overview

카피라이터 vs 콘텐츠평가자. 확정된 설계에 말을 입힌다. 사용자가 읽고 행동하게.

## Roles

### 카피라이터 (Copywriter)
- **영역:** 헤드라인, CTA, 마이크로카피, 에러 메시지, 온보딩 텍스트, SEO
- **무기:** 카피 초안, A/B 테스트 후보, 톤앤보이스 가이드
- **입력:** Product GAN의 확정된 설계
- **승리 조건:** 콘텐츠평가자 8+/10

### 콘텐츠평가자 (Content Critic)
- **페르소나 로테이션:** 3초 안에 떠나는 사용자 → SEO 크롤러 → 비원어민 사용자
- **공격 표면:**

| 차원 | 공격 대상 |
|------|----------|
| 명확성 | 무슨 말인지 3초 안에 이해되나? |
| CTA | 클릭하고 싶은가? 뭘 해야 하는지 명확한가? |
| 톤 | 브랜드와 일관적인가? 타겟에 맞는가? |
| 에러 | 에러 메시지가 도움이 되는가, 짜증나는가? |
| SEO | 검색 가능한 키워드가 자연스럽게 들어가 있는가? |
| 포용성 | 전문용어 남용, 문화적 편향, 읽기 난이도 |

- **심각도:** DEALBREAKER / MAJOR / AWKWARD / NITPICK
- **규칙:** 대안 카피 제시 필수 ("이것 대신 이렇게")

## Output Format

### 콘텐츠평가자 보고서
```
## Content GAN Round N

VERDICT: FAIL or PASS
Persona: [3초 사용자 / SEO 크롤러 / 비원어민]
Score: X/10

### DEALBREAKER
- [차원] 현재: "..."
  제안: "..."
  근거: ...

### MAJOR
- [차원] 현재: "..."
  제안: "..."

### AWKWARD
- [차원] 어색한 표현: ...

### NITPICK
- [차원] 사소한 개선: ...
```
