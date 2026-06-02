---
name: debug
description: Systematic debugging workflow — reproduce, isolate, hypothesize, verify, fix
---

You are running a structured debugging session. Follow this protocol exactly:

**PHASE 1 — REPRODUCE**
Before touching any code:
1. Confirm you can reproduce the bug consistently
2. Identify the exact inputs/conditions that trigger it
3. Identify what the expected behavior is vs. what actually happens
4. Note the environment: Python version, OS, relevant package versions

**PHASE 2 — ISOLATE**
Narrow the blast radius:
1. Read the full stack trace if one exists — start from the BOTTOM (the actual failure), not the top
2. Identify the smallest code path that triggers the issue
3. Check: is this a data problem, a logic problem, or an integration problem?
4. Add strategic `print()` or `logging.debug()` statements at boundaries — not throughout

**PHASE 3 — HYPOTHESIZE**
Form ranked hypotheses before looking at code:
1. State your #1 hypothesis in one sentence
2. State what evidence would confirm or refute it
3. Only then look at the specific lines implicated

**PHASE 4 — VERIFY**
- Write a minimal reproduction case (ideally <20 lines)
- Confirm the hypothesis is correct before fixing
- If hypothesis is wrong, return to Phase 2 — don't thrash

**PHASE 5 — FIX**
- Fix the root cause, not the symptom
- Do not add `try/except` to hide an error — understand why it happens
- Write one test that would have caught this bug
- State clearly: what was wrong, why it happened, what the fix does

**OUTPUT FORMAT**
Present your findings as:
```
ROOT CAUSE: [one sentence — what was actually wrong]
LOCATION: [file:line]
WHY IT HAPPENED: [2–3 sentences — the chain of events leading to the bug]
FIX: [the specific change made]
PREVENTION: [one test or guard that would catch this class of bug in future]
```

Ask the user for the error message, stack trace, and relevant code before starting. Do not guess without seeing the actual error.
