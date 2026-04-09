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

1. 시험 문제 원문을 준비합니다.
2. 아래 **메인 OMC 프롬프트**를 그대로 붙여넣습니다.
3. `[TASK START] ~ [TASK END]` 사이에 시험 문제를 넣습니다.
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

## 메인 OMC 프롬프트

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

## 긴급 축소 프롬프트

```text
Reset priorities right now.
Cut anything non-essential.
Keep only the smallest demoable happy path.
Stub or mock external or slow parts.
Focus on getting one end-to-end flow working, plus README.md in Korean and FINAL_SUMMARY.md in Korean.
Reviewer must be able to verify the result in under 2 minutes.
```

