---
name: chain-of-thought
description: Apply chain-of-thought prompting to any reasoning task — select the right CoT variant, write the prompt, and validate the reasoning chain
---

You are applying chain-of-thought (CoT) prompting to improve a model's reasoning on a task.

**Step 1 — Understand the task**

Ask for (or extract):
1. What reasoning task needs CoT? (math, logic, classification, planning, code debugging)
2. What model is being used? (Claude Opus/Sonnet, GPT-4, etc.)
3. Is this zero-shot or do examples help? (does the user have worked examples?)
4. What's the failure mode being solved? (wrong answers, inconsistent logic, skipped steps)

**Step 2 — Select the CoT variant**

| Variant | When to use |
|---|---|
| **Zero-shot CoT** | Add "Think step by step." — simple, effective for most tasks |
| **Few-shot CoT** | Provide 2-5 worked examples with reasoning chains — best for consistent format |
| **Self-consistency** | Sample N reasoning paths, majority-vote the answer — high-stakes decisions |
| **Tree of Thoughts** | Explore multiple reasoning branches — open-ended planning tasks |
| **ReAct** | Interleave reasoning + tool calls — agent tasks with external lookups |
| **Scratchpad** | Give model a dedicated thinking block before final answer — structured output tasks |

**Step 3 — Write the prompt**

**Zero-shot CoT template:**
```
[Task description]

Think through this step by step before giving your final answer.

[Input]
```

**Few-shot CoT template:**
```
[Task description]

Here are some examples:

Example 1:
Input: [example input]
Reasoning: [Step 1: ... Step 2: ... Step 3: ...]
Answer: [answer]

Example 2:
Input: [example input]
Reasoning: [Step 1: ... Step 2: ...]
Answer: [answer]

Now solve this:
Input: [actual input]
Reasoning:
```

**Scratchpad / structured thinking template (for Claude with extended thinking):**
```
[Task description]

Before answering, work through your reasoning in a <thinking> block.
Then provide your final answer in an <answer> block.

Format:
<thinking>
[your step-by-step reasoning]
</thinking>
<answer>
[final answer]
</answer>

[Input]
```

**Step 4 — Validate the reasoning chain**

Checks to apply to model output:
- Does each step follow logically from the previous?
- Are there unsupported leaps in logic?
- Does the conclusion actually follow from the reasoning shown?
- For math: verify each arithmetic step independently
- For classification: does the stated rationale match the assigned label?

**Step 5 — Output**

```
CHAIN-OF-THOUGHT DESIGN — [Task Name]
─────────────────────────────────────────────────────────────
TASK TYPE:    [math / logic / classification / planning / code]
VARIANT:      [zero-shot / few-shot / self-consistency / ReAct]
FAILURE MODE: [what was going wrong before CoT]

PROMPT (ready to use):
─────────────────────────────────────────────────────────────
[Full prompt text]
─────────────────────────────────────────────────────────────

EXAMPLE OUTPUT (expected reasoning chain):
[Show what a good CoT trace looks like for this task]

VALIDATION CHECKLIST
  □ Steps are explicit and numbered
  □ No unexplained jumps between steps
  □ Final answer directly derived from last step
  □ [Task-specific check]

IMPROVEMENT OPTIONS
  If accuracy is still low:
  → Add more few-shot examples (target: 3-5)
  → Use self-consistency: sample 5 paths, take majority answer
  → Decompose into sub-problems before applying CoT
```
