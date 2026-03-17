# ez-harness 전체 구조 및 홍보 전략

## 무엇인가

AI 코드 생성 후 반복되는 실패 패턴을 구조적으로 차단하는 규칙 파일 시스템.

Claude Code + Antigravity(Google) 양쪽에서 동작.

### 왜 필요한가

CodeRabbit 2026 리포트 기준:
- AI 생성 코드 전체 이슈: 인간 대비 **1.7배**
- 보안 취약점: **2.74배**
- 에러 핸들링 미흡: **2배**

속도는 빠르지만 검증 없이 쓰면 더 많은 버그를 만든다.

---

## 시스템 구성

| 단계 | 이름 | 역할 | 트리거 |
|------|------|------|--------|
| 0 | 역추적 | 버그 근본 원인 확정 | 에러 키워드 감지 |
| 1 | GATE | 버전 + 패턴 로드 | 코드 생성 전 1회 |
| 2 | DMAD 토론 | 설계자 vs 사용자 문답 | 3+ 파일 변경 시 |
| 3 | sim | 장기 실패 시뮬레이션 | 3+ 파일 변경 후 자동 |
| 4 | smartLoop | 실패 유형별 최소 지점 재시작 | sim/Check 실패 시 |

### 핵심: sim (장기 실패 시뮬레이션)

가장 큰 차별점. 아직 발생하지 않은 버그를 배포 전에 잡는다.

```
S1. "3개월 후 + 10배 규모" 상황에서 코드를 한 줄씩 따라가며 터지는 곳 찾기
S2. 근본 원인 루프 — "이걸 고치려면 다른 걸 먼저 고쳐야 하는가?" → 최하위 레이어까지
S3. 수정 위치 강제 검증 (S2 근본 원인 위치 ≠ S3 수정 위치이면 수정 금지)
    fallback 패턴 금지: 증상 우회 수정 차단
```

코드를 잘 모르는 상태에서 AI에게 맡기는 경우에도 유효.
AI가 표면 증상만 고치고 끝내는 패턴을 구조적으로 차단.

---

## 파일 구조

```
ez-harness/
├── claude-code/            # Claude Code 버전
│   ├── CLAUDE.md           # 설치 시작점
│   ├── rules/
│   │   └── eazycheck-v5.md # GATE + DMAD + sim + Check
│   ├── skills/
│   │   └── sim/SKILL.md    # /sim 스킬
│   └── state/
│       └── routing.json    # 에이전트 라우팅
│
└── antigravity/            # Antigravity(Google) 버전
    ├── RULES.md            # 전역 규칙 (자동 로드)
    └── .agents/
        ├── workflows/sim.md
        └── skills/sim/SKILL.md
```

---

## 대상 사용자

**1차 타겟: 바이브코더 (비개발자)**
- 코드를 잘 모르는 상태에서 AI에게 맡기는 사람
- AI가 뭘 고치는지 알 수 없어서 표면 수정인지 근본 수정인지 판단 불가
- ez-harness가 구조적으로 표면 수정을 차단하므로 가장 효과가 큼

**2차 타겟: 개발자 + AI 헤비유저**
- AI 코드 리뷰 자동화에 관심 있는 개발자
- Claude Code / Antigravity 사용자

---

## 홍보 채널별 전략

### GitHub (현재 활성)
- 저장소: github.com/ez-claude/ez-harness
- 소개 페이지: ez-claude.github.io/ez-harness/ez-harness.html
- 강의 배포: lecture-week3.vercel.app

### 국내

| 채널 | 포맷 | 핵심 메시지 |
|------|------|------------|
| X(트위터) | 짧은 쓰레드 | "AI 보안 취약점 2.74배" + 스크린샷 |
| velog/tistory | 블로그 포스트 | 설치 방법 + 실제 sim 결과 |
| 오픈카톡 (Claude, AI개발) | 링크 공유 | ez-harness.html 링크 |
| 패스트캠퍼스/인프런 커뮤니티 | 댓글/포스트 | 강의 수강생 대상 |

### 해외

| 채널 | 포맷 | 핵심 메시지 |
|------|------|------------|
| Reddit r/ClaudeAI | 텍스트 포스트 | "Rules file system that catches bugs before they happen" |
| Reddit r/LocalLLaMA | 기술 설명 | sim deep 방법론 설명 |
| HackerNews | Show HN | 데이터 기반 (CodeRabbit 통계) |
| X(English) | 쓰레드 | Claude Code / Antigravity 태그 |

### 가장 빠른 경로
1. X 쓰레드 (한글) — CodeRabbit 통계 + sim 결과 스크린샷
2. r/ClaudeAI 포스트 (영문) — "Show HN" 스타일
3. 강의 수강생에게 먼저 검증

---

## 포지셔닝

| 항목 | 내용 |
|------|------|
| 카테고리 | AI Harness (OpenAI/Toss/Martin Fowler가 정의한 개념) |
| 차별점 | 사후 디버깅 아닌 **사전 시뮬레이션** |
| 키워드 | Claude Code rules, CLAUDE.md, AI harness, vibe coding |
| 경쟁 없음 | "운영 체계" 교육/도구 시장 현재 공백 |

---

## 연관 저장소

- [ez-harness](https://github.com/ez-claude/ez-harness): 이 파일
- [ez-plans](https://github.com/ez-claude/ez-plans): 플랜 파일 영속화
- [lectures](https://github.com/ez-claude/lectures): 강의 슬라이드
