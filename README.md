# krafton-ai-fde-2h-playbook

크래프톤 AI FDE 2시간 현장 테스트에서 **MVP 이상 결과물**을 안정적으로 만들기 위한 OMC(oh-my-claudecode) 실전용 플레이북입니다.

핵심 목표는 하나입니다.

> **2시간 안에 심사자가 2분 내 검증 가능한 결과물을 만든다.**

---

## 목적

이 문서는 아래 상황을 전제로 합니다.

- 현장 코딩 테스트
- 제한 시간 2시간
- AI 도구 사용 가능
- 완성도보다 **실행 가능성, 검증 가능성, 전달력**이 중요
- OMC를 이용해 **과설계 없이 빠르게 MVP+ 수준까지 도달**하는 것이 목표

---

## 사용 방법

시험 시작 직후 아래 순서대로 진행합니다.

1. 시험 문제 원문을 확인합니다.
2. 문제가 애매하거나 요구사항 해석이 중요하면 아래 **딥 인터뷰 프롬프트**를 먼저 사용합니다.
3. 그다음 아래 **메인 OMC 프롬프트**를 사용해 바로 구현에 들어갑니다.
4. 진행이 꼬이거나 시간이 밀리면 아래 **긴급 축소 프롬프트**를 추가로 넣습니다.

---

## 운영 원칙

- 처음부터 병렬화하지 않는다.
- 처음 10분 안에 요구사항을 압축하고 범위를 자른다.
- 핵심 해피패스 1개를 먼저 끝까지 연결한다.
- 외부 연동, 복잡한 의존성, 예쁜 구조보다 **돌아가는 결과물**을 우선한다.
- 90분 이후에는 새 기능보다 **안정화, README, 최종 정리**에 집중한다.
- 문서화는 한국어로 하되, 코드/파일명/기술 식별자는 영어로 유지한다.

---

## 1) 딥 인터뷰 프롬프트

문제가 모호하거나 요구사항 해석이 중요할 때 먼저 사용하는 프롬프트입니다.  
목표는 코딩 전에 **범위를 압축하고 평가 리스크를 드러내는 것**입니다.

```text
/deep-interview "
I am taking a 2-hour onsite Krafton AI FDE test.
Before coding, clarify the task for fastest MVP delivery.

Task:
[여기에 시험 문제 원문 전체 붙여넣기]

Your job:
- Identify must-have requirements
- Identify nice-to-have requirements
- Identify likely non-goals for a 2-hour attempt
- Surface hidden assumptions
- Highlight evaluation-risk areas
- Recommend the smallest demoable happy path
- Keep the result concise and execution-oriented
- Output in Korean
"
```

### 딥 인터뷰를 쓰는 타이밍

아래 중 하나라도 해당되면 먼저 쓰는 편이 안전합니다.

- 문제 문장이 애매하다
- 요구사항이 여러 갈래로 열려 있다
- 구현보다 해석이 더 중요해 보인다
- 무엇을 버려야 할지 애매하다
- 평가 포인트가 기능 수가 아니라 판단력일 가능성이 높다

---

## 2) 메인 OMC 프롬프트

아래 프롬프트를 시험 시작 직후 그대로 사용합니다.

```text
ralph: You are my execution lead for a 2-hour onsite Krafton AI FDE test.
Your goal is not elegance. Your goal is a working, reviewable MVP-plus result within the time limit.

Read the task below and execute aggressively.

[TASK START]
여기에 시험 문제 원문 전체 붙여넣기
[TASK END]

Core mission:
- Optimize for a working demo, not completeness.
- Do not ask unnecessary questions.
- If something is ambiguous, make the most reasonable assumption, state it briefly, and continue.
- Prefer the simplest architecture and the fewest dependencies.
- Prefer local, deterministic, mockable solutions over fragile external integrations.
- If blocked for more than 5 minutes, choose the simpler fallback path immediately.
- Do not gold-plate.
- Do not refactor unless it directly helps delivery.
- Keep core interfaces stable once the first vertical slice is defined.
- Reviewer should be able to verify the result in under 2 minutes.
- Optimize everything for that constraint.
- Do not wait for my approval. Make reasonable assumptions and execute.

Language rules:
- All reviewer-facing documentation must be written in Korean.
- README.md must be written in Korean.
- FINAL_SUMMARY.md must be written in Korean.
- Demo steps, setup instructions, execution notes, and user-facing explanations must be written in Korean.
- Do not write reviewer-facing documentation in English.
- Keep code, variable names, file names, function names, API paths, schema names, and technical identifiers in English unless Korean is clearly better.

Success criteria:
1. One end-to-end happy path works and can be demonstrated in under 2 minutes.
2. The project can be run with minimal setup.
3. Obvious invalid inputs are handled cleanly.
4. There is a concise README in Korean with setup, run, and demo steps.
5. There is a final summary in Korean of:
   - what works,
   - what is stubbed or mocked,
   - what was intentionally omitted,
   - next improvements if given more time.

Required execution sequence:

Phase 1: Compress the problem
- Extract:
  - Must-have requirements
  - Nice-to-have requirements
  - Non-goals for this 2-hour attempt
- Choose the smallest shippable scope.
- Output this briefly and move on immediately.

Phase 2: Freeze the minimum design
- Define the minimum architecture and interfaces.
- Identify the single most important vertical slice.
- Start implementation immediately after freezing the minimum interfaces.

Phase 3: Build the vertical slice first
- Implement the smallest end-to-end path first.
- Get it running as early as possible.
- Do not expand scope until this path works.

Phase 4: Stabilize
- Add only essential validation, error handling, and one or two critical edge cases.
- Add a smoke test or verification script if possible.
- Produce README.md in Korean with exact commands.
- Make the demo path obvious.

Phase 5: Polish only after demo safety is secured
- Improve naming, logs, UX text, and tiny issues only if the core demo is already safe.

Parallelization rule:
- Do NOT parallelize at the beginning.
- Only after the first end-to-end slice works, spawn at most 3 workers for isolated tasks like:
  1. README/demo docs
  2. smoke test / verification
  3. input validation / error messages
- Those workers must not break core interfaces.

Time strategy:
- T+0 to T+10: compress scope and freeze minimum design
- T+10 to T+60: build the core vertical slice
- T+60 to T+90: stabilize and verify
- T+90 to T+120: no new major features; focus only on demo reliability, README, summary, and bug fixes

Output style:
- Be concise and execution-focused.
- After each milestone, report only:
  - current status,
  - what works,
  - current blocker if any,
  - next immediate action

Hard constraints:
- No broad rewrites
- No speculative architecture
- No unnecessary tests beyond smoke-level unless trivial
- No deep dependency setup unless absolutely required
- No hidden incomplete state
- Always leave the repo runnable

Fallback rule:
- If progress is slow, cut scope immediately.
- Stub or mock external, slow, or unreliable parts.
- Keep only the smallest demoable happy path.
- Prioritize runnable code + Korean README + Korean FINAL_SUMMARY over extra features.

Final deliverables before stopping:
- runnable code
- README.md
- optional smoke test / verification script
- FINAL_SUMMARY.md

Begin now.
```

---

## 3) 긴급 축소 프롬프트

진행이 꼬이거나 시간이 밀릴 때 바로 추가로 넣는 프롬프트입니다.

아래 상황에서 특히 유용합니다.

- 40~50분이 지났는데 핵심 흐름이 아직 안 붙음
- 기능은 많은데 어느 것도 안정적으로 데모가 안 됨
- 외부 연동, 의존성, 예외 처리 때문에 구현 속도가 급격히 느려짐
- 제출 직전인데 구현만 늘어나고 정리는 안 됨

```text
Reset priorities right now.
Cut anything non-essential.
Keep only the smallest demoable happy path.
Stub or mock external or slow parts.
Focus on getting one end-to-end flow working, plus README.md in Korean and FINAL_SUMMARY.md in Korean.
Reviewer must be able to verify the result in under 2 minutes.
```

---

## 추천 사용 흐름

### A. 문제가 명확할 때

1. 메인 OMC 프롬프트 사용
2. 구현 진행
3. 필요하면 긴급 축소 프롬프트 사용

### B. 문제가 애매할 때

1. 딥 인터뷰 프롬프트 사용
2. 딥 인터뷰 결과로 범위 압축
3. 메인 OMC 프롬프트 사용
4. 필요하면 긴급 축소 프롬프트 사용

---

## 실전 운영 팁

### 1. 시작 10분 안에 범위를 잘라야 한다

문제를 받으면 모든 요구사항을 다 구현하려고 하면 시간이 무너집니다.  
반드시 아래 세 가지로 나눕니다.

- 반드시 돌아가야 하는 핵심 기능
- 있으면 좋은 기능
- 이번 2시간에는 버릴 기능

### 2. 처음부터 병렬화하지 않는다

초반 병렬화는 빨라 보이지만 실제로는 충돌과 재작업이 늘어나는 경우가 많습니다.  
먼저 **세로 한 줄(vertical slice)** 을 완성한 뒤, 문서화·검증·입력 검증만 분리하는 편이 안전합니다.

### 3. 외부 연동은 쉽게 버릴 수 있어야 한다

API, 인증, 크롤링, 클라우드 연결, 복잡한 DB 설정처럼 시간을 먹는 요소는  
조금만 막혀도 **stub/mock/local fallback** 으로 전환하는 것이 낫습니다.

### 4. 심사자는 코드보다 검증 가능성을 먼저 본다

아무리 코드를 많이 짜도 실행 방법이 불분명하면 가치가 크게 떨어집니다.  
적어도 아래는 반드시 남아야 합니다.

- 어떻게 실행하는지
- 어떤 기능이 동작하는지
- 어떤 것은 mock/stub인지
- 무엇을 일부러 생략했는지

### 5. 90분 이후에는 새 기능을 멈춘다

이 시점부터는 기능 추가보다 아래가 더 중요합니다.

- 데모 실패 방지
- README 정리
- 실행 명령 확인
- 에러 메시지 최소 정리
- 최종 요약 작성

---

## 권장 제출 상태

최종적으로는 아래 상태를 목표로 잡습니다.

- 저장소가 실행 가능함
- 핵심 해피패스 1개가 동작함
- README에 실행/데모 방법이 한국어로 정리됨
- FINAL_SUMMARY에 동작 범위와 생략 범위가 정리됨
- 외부 연동이 불안정하면 stub/mock임을 명시함

---

## 가장 중요한 한 줄

> **완전한 결과물보다, 심사자가 2분 안에 검증할 수 있는 돌아가는 결과물이 더 강하다.**

---

## 추천 레포 구성

```text
krafton-ai-fde-2h-playbook/
└─ README.md
```
