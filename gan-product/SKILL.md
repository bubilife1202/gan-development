---
name: product
description: Use when designing or improving product UX - adversarial loop between Designer and Product Critic. Covers UX/UI, usability, user flows, onboarding, empty states, accessibility, mobile responsiveness.
---

# Product GAN

## Overview

생성자 vs 제품평가자. 확정된 사업모델을 제품으로 설계한다.

## Round Protocol

```
Round N:
  1. Builder 제출 (설계안 + Scorecard 지표 대응표)
  2. Critic 공격 (심각도별 분류 + 유저 시나리오 + "잘하면 이렇다")
  3. Builder 반박/수용 선언 (항목별 + 증거)
  4. Critic 재채점 (rubric 기반)
  5. PASS(8+) → 확정 설계 + 지표 달성 증빙 → Code GAN으로 핸드오프
     FAIL → Round N+1 (최대 4라운드)
     Deadlock → 미해결 항목 + 리스크 태그 달고 진행
```

## Handoff: Business GAN → Product GAN

Product GAN은 Business GAN의 확정 산출물을 입력으로 받는다:
- 사업모델 문서
- 타겟 페르소나
- **Scorecard의 Product GAN 판정 기준**

매 라운드 제출 시, Builder는 Scorecard 지표에 대한 달성 상태를 명시해야 한다.

## Roles

### 생성자 (Designer)
- **영역:** UX, 플로우, 정보구조, 와이어프레임, 인터랙션
- **무기:** 플로우차트, 와이어프레임, 유저저니맵
- **입력:** Business GAN의 확정 산출물
- **제출 의무:**
  ```
  ## Builder Round N 제출

  ### 설계안
  (변경사항 + 근거)

  ### Scorecard 지표 대응
  - 핵심 전환까지 클릭 수: 현재 N → 목표 M (달성/미달성)
  - 온보딩 완료율 추정: N% (근거: ...)

  ### Critic 피드백 대응 (Round 2+)
  - [수용] 항목명 → 수정 내용
  - [기각] 항목명 → 기각 사유 + 반박 증거 (유저 테스트, 경쟁사 사례, 휴리스틱 분석)
  ```
- **기각 규칙:** 증거 없는 기각은 수용으로 간주.
- **승리 조건:** 제품평가자 8+/10

### 제품평가자 (Product Critic)
- **페르소나 로테이션:** Round 1 첫 방문 사용자 → Round 2 파워 유저 → Round 3+ 접근성 감사관
- **공격 표면:**

| 차원 | 공격 대상 |
|------|----------|
| UX/UI | 네비게이션, 인지 부하, 시각적 위계, 반응형 |
| 사용성 | 온보딩 마찰, 에러 복구, 접근성, 빈/로딩 상태 |
| 플로우 | 핵심 전환 경로, 이탈 지점, 데드엔드 |
| 심리 | 동기 부여, 습관 형성, 보상 타이밍, 사회적 증거 |
| 일관성 | 패턴 통일, 예측 가능성, 학습 곡선 |

- **심각도:** DEALBREAKER / MAJOR / FRICTION / POLISH
- **규칙:** 유저 시나리오 필수 + "잘하면 이렇다" 필수 + Scorecard 지표 위반 시 자동 MAJOR

## Scoring Rubric

| 점수 | 기준 |
|------|------|
| 9-10 | DEALBREAKER 0, MAJOR 0, Scorecard 지표 전부 달성 |
| 7-8 | DEALBREAKER 0, MAJOR ≤1 (수용됨), Scorecard 핵심 지표 달성 |
| 5-6 | DEALBREAKER 0, MAJOR 2+ 미해결 |
| 3-4 | DEALBREAKER 1+ 미해결 또는 Scorecard 지표 대부분 미달 |
| 1-2 | 핵심 플로우가 작동하지 않음 |

PASS = 8+.

## Output Format

### 제품평가자 보고서
```
## Product GAN Round N

VERDICT: FAIL or PASS
Persona: [첫 방문 사용자 / 파워 유저 / 접근성 감사관]
Score: X/10
Rubric 근거: DEALBREAKER N개, MAJOR N개, Scorecard 달성 N/M

### DEALBREAKER
- [차원] 문제. 유저 시나리오: "나는..."
  잘하면 이렇다: ...

### MAJOR
- [차원] 문제. 리텐션/전환 영향: ...

### FRICTION
- [차원] 불편한 순간: ...

### POLISH
- [차원] 있으면 좋은 것

### Scorecard 검증
- [지표명] 목표: N, 현재: M → PASS/FAIL
```
