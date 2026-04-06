---
name: product
description: Use when designing or improving product UX - adversarial loop between Product Designer and Product Critic. Covers UX/UI, usability, user flows, onboarding, empty states, accessibility, mobile responsiveness.
---

# Product GAN

## Overview

제품설계자 vs 제품평가자. 확정된 사업모델을 제품으로 설계한다.

## Round Protocol

```
Round N:
  1. Builder 제출 (설계안 + Scorecard 지표 대응표 + 결정ID)
  2. Critic 공격 (심각도별 + 유저 시나리오 + 증거 + "잘하면 이렇다")
  3. Builder 반박/수용 선언 (항목별 + 증거)
  4. Critic 재채점 (rubric 산식 적용)
  5. PASS(8+) → 확정 설계 패키지 → Code/Design GAN으로 핸드오프
     FAIL → Round N+1 (최대 4라운드)
     Deadlock → STOP. 사용자에게 판단 요청. 자동 진행 금지.
```

## Handoff: Business GAN → Product GAN

Product GAN은 Business GAN의 확정 산출물 전체를 입력으로 받는다:
- 사업모델 문서 (결정ID 포함)
- 타겟 페르소나
- Scorecard의 Product GAN 판정 기준
- **리스크 레지스터** (있으면 반드시 설계에 반영)

매 라운드 제출 시, Builder는 Scorecard 지표 + 리스크 대응 상태를 명시해야 한다.

## Roles

### 제품설계자 (Product Designer)
- **영역:** UX, 플로우, 정보구조, 와이어프레임, 인터랙션
- **무기:** 플로우차트, 와이어프레임, 유저저니맵
- **입력:** Business GAN 확정 산출물
- **제출 의무:**
  ```
  ## Builder Round N 제출

  ### 설계안
  - [PRD-001] 변경사항 + 근거 (참조: BIZ-00N)
  - [PRD-002] ...

  ### Scorecard 지표 대응
  - [SC-P01] 지표명: 현재값 → 목표값 (측정방법: ...) → PASS/FAIL
  - [SC-P02] ...

  ### 리스크 레지스터 대응
  - [RISK-001] → 설계에서 이렇게 완화: ...

  ### Critic 피드백 대응 (Round 2+)
  - [수용] 항목명 → 수정 내용
  - [기각] 항목명 → 기각 사유 + 증거 (경쟁사 사례/휴리스틱 분석/유저 테스트 결과)
  ```
- **결정ID 규칙:** `PRD-001`, `PRD-002` 형태. Business 결정을 참조할 때 `(참조: BIZ-00N)` 명시.
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
- **규칙:**
  - 유저 시나리오 필수 ("나는 X를 하려고 했는데...")
  - "잘하면 이렇다" 필수 (경쟁사 사례 또는 디자인 원칙 인용)
  - **증거 필수**: 경쟁사 스크린샷, 닐슨 휴리스틱, WCAG 기준, 또는 사용성 원칙 인용. 감상만으로는 지적 불가.
  - Scorecard 지표 위반 시 자동 MAJOR

## Scoring Rubric

산식: `기본 10점 - (DEALBREAKER × 4) - (미해결 MAJOR × 1.5) - (Scorecard FAIL × 1)`

| 점수 | 산식 결과 |
|------|----------|
| 9-10 | 9.0+ |
| 8 | 8.0-8.9 |
| 7 | 7.0-7.9 |
| 5-6 | 5.0-6.9 |
| 3-4 | 3.0-4.9 |
| 1-2 | <3.0 |

PASS = 8.0+.

## 확정 산출물 (Handoff Package)

PASS 시 아래를 반드시 포함:

```
## Product GAN 확정 설계

### 1. 유저 플로우
- 핵심 전환 경로: 진입 → ... → 목표 행동 (스텝 수: N)
- 보조 플로우: ...

### 2. 페이지/컴포넌트 목록
- [PRD-001] 페이지/컴포넌트명 — 목적 — 포함 요소
- [PRD-002] ...

### 3. 정보 구조
- 네비게이션 구조
- 각 페이지의 필수 콘텐츠 항목

### 4. Scorecard 달성 증빙
- [SC-P01] 최종값: N (목표: M) → PASS

### 5. 리스크 레지스터 (갱신)
- [RISK-001] 상태: 완화됨/미완화 → 이유
- (새로 발견된 리스크 추가)

### 6. Business 결정 추적
- PRD-001 ← BIZ-001
- PRD-002 ← BIZ-003
```

## Output Format

### 제품평가자 보고서
```
## Product GAN Round N

VERDICT: FAIL or PASS
Persona: [첫 방문 사용자 / 파워 유저 / 접근성 감사관]
Score: X/10 (산식: 10 - DB×4 - MAJOR×1.5 - SC_FAIL×1 = X)

### DEALBREAKER
- [PRD-00N][차원] 문제. 유저 시나리오: "나는..."
  증거: [경쟁사/원칙/WCAG]
  잘하면 이렇다: ...

### MAJOR
- [PRD-00N][차원] 문제. 증거: ...

### FRICTION
- [차원] 불편한 순간. 증거: ...

### POLISH
- [차원] 있으면 좋은 것

### Scorecard 검증
- [SC-P01] 목표: N, 현재: M, 측정방법: ... → PASS/FAIL
```
