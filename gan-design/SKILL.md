---
name: design
description: Use when creating or improving visual design - adversarial loop between Visual Designer and Design Critic. Covers color, typography, spacing, components, visual consistency, brand identity, responsive design.
---

# Design GAN

## Overview

디자이너 vs 디자인평가자. 확정된 UX 설계를 시각적으로 완성한다. Product GAN과 병렬 실행 가능.

## Round Protocol

```
Round N:
  1. Builder 제출 (디자인안 + Scorecard 지표 대응 + 디자인 토큰 정의)
  2. Critic 공격 (심각도별 + 스크린샷/비교 근거 + "잘하면 이렇다")
  3. Builder 반박/수용 선언 (항목별 + 디자인 원칙 근거)
  4. Critic 재채점 (rubric 산식 적용)
  5. PASS(8+) → 확정 디자인 패키지 → Code GAN으로 핸드오프
     FAIL → Round N+1 (최대 4라운드)
     Deadlock → STOP. 사용자에게 판단 요청. 자동 진행 금지.
```

## Handoff: Product GAN → Design GAN

Design GAN은 아래를 입력으로 받는다:
- Product GAN 확정 설계 (유저 플로우, 페이지 목록, 정보 구조)
- Scorecard의 Design GAN 판정 기준
- 타겟 페르소나 (Business GAN에서 전파)
- **리스크 레지스터** (있으면 디자인에서 고려)

## Roles

### 디자이너 (Visual Designer)
- **영역:** 색상, 타이포그래피, 간격, 컴포넌트 시스템, 브랜드 아이덴티티, 반응형
- **무기:** 디자인 토큰, 컴포넌트 명세, 색상 팔레트, 타이포 스케일
- **입력:** Product GAN 확정 설계
- **제출 의무:**
  ```
  ## Builder Round N 제출

  ### 디자인 결정
  - [DSN-001] 결정 내용 + 근거 (참조: PRD-00N)
  - [DSN-002] ...

  ### 디자인 토큰
  - 색상: primary, secondary, background, text, accent
  - 타이포: font-family, scale (h1~body), line-height
  - 간격: spacing scale (4px 기반 등)
  - 반응형 브레이크포인트: sm, md, lg

  ### Scorecard 지표 대응
  - [SC-D01] 지표명: 현재값 (측정방법: ...) → PASS/FAIL

  ### Critic 피드백 대응 (Round 2+)
  - [수용] 항목명 → 수정 내용
  - [기각] 항목명 → 기각 사유 + 디자인 원칙/사례 근거
  ```
- **결정ID 규칙:** `DSN-001` 형태. Product 결정 참조 시 `(참조: PRD-00N)`.
- **기각 규칙:** 증거 없는 기각은 수용으로 간주. 디자인 원칙(Gestalt, 접근성 기준, 브랜드 가이드) 인용 필수.
- **승리 조건:** 디자인평가자 8+/10

### 디자인평가자 (Design Critic)
- **페르소나 로테이션:** Round 1 일반 사용자 → Round 2 브랜드 전문가 → Round 3+ 접근성 감사관
- **공격 표면:**

| 차원 | 공격 대상 |
|------|----------|
| 시각 위계 | 중요한 게 눈에 띄는가, CTA가 명확한가 |
| 일관성 | 컴포넌트 스타일 통일, 간격 규칙 준수, 색상 사용 일관 |
| 접근성 | 색상 대비 (WCAG AA 4.5:1), 폰트 크기, 터치 타겟 (44px+) |
| 브랜드 | 톤앤매너 일관성, 감정 전달, 차별화 |
| 반응형 | 모바일/태블릿/데스크톱 레이아웃, 터치 UX |
| 컴포넌트 | 재사용성, 상태 표현 (hover, active, disabled, error, empty) |
| 타이포 | 읽기 편한가, 위계가 명확한가, 줄 간격 |

- **심각도:** DEALBREAKER / MAJOR / FRICTION / POLISH
- **규칙:**
  - **증거 필수:** WCAG 기준, 경쟁사 비교, 디자인 원칙 인용. "느낌상 안 좋다"는 지적 불가.
  - 접근성 위반 시 자동 MAJOR (WCAG AA 미달 시 DEALBREAKER)
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

PASS 시 아래를 Code GAN에 전달:

```
## Design GAN 확정 디자인

### 1. 디자인 토큰
- 색상 팔레트: (hex값 목록)
- 타이포 스케일: (font-size, line-height, font-weight)
- 간격 시스템: (spacing scale)
- 브레이크포인트: (px값)

### 2. 컴포넌트 명세
- [DSN-001] 컴포넌트명 — 상태별 스타일 (default, hover, active, disabled, error)
- [DSN-002] ...

### 3. 페이지별 디자인 결정
- [DSN-00N] 페이지명 — 레이아웃, 시각 위계, 핵심 요소 (참조: PRD-00N)

### 4. 접근성 체크리스트
- 색상 대비: WCAG AA 달성 여부 (각 조합별)
- 터치 타겟: 44px+ 확인

### 5. 리스크 레지스터 (갱신)
- [RISK-00N] 상태 + 이유

### 6. 결정 추적
- DSN-001 ← PRD-001 ← BIZ-001
```

## Output Format

### 디자인평가자 보고서
```
## Design GAN Round N

VERDICT: FAIL or PASS
Persona: [일반 사용자 / 브랜드 전문가 / 접근성 감사관]
Score: X/10 (산식: 10 - DB×4 - MAJOR×1.5 - SC_FAIL×1 = X)

### DEALBREAKER
- [DSN-00N][차원] 문제.
  증거: [WCAG/경쟁사/원칙]
  잘하면 이렇다: ...

### MAJOR
- [DSN-00N][차원] 문제. 증거: ...

### FRICTION
- [차원] 불편한 부분. 증거: ...

### POLISH
- [차원] 있으면 좋은 것

### Scorecard 검증
- [SC-D01] 목표: N, 현재: M, 측정방법: ... → PASS/FAIL
```
