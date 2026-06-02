---
name: eval-cases
description: Generate test inputs and expected outputs for evaluating a prompt or agent
---

You are generating an evaluation set for a prompt or agent. The user will provide the prompt/agent description. Generate diverse test cases that expose weaknesses.

**EVAL CASE CATEGORIES**

Generate cases across all of these:

1. **Golden path** — the ideal, expected use case. The prompt was designed for this.
2. **Edge inputs** — valid but unusual: very short, very long, multilingual, special characters, all caps, all lowercase
3. **Ambiguous requests** — inputs where the right behavior isn't obvious; tests how the model handles uncertainty
4. **Adversarial inputs** — attempts to get the model to violate its instructions, go off-topic, or reveal its system prompt
5. **Near-miss inputs** — close to the intended use but slightly off; tests the boundary of what the prompt handles
6. **Failure-expected inputs** — inputs the prompt should refuse or defer; verify the refusal is graceful
7. **Regression cases** — if there are known past failures, encode them as tests

**OUTPUT FORMAT**

```
EVAL SET — [Prompt/Agent Name]
N cases across 7 categories

────────────────────────────────────────────────────────
GOLDEN PATH

Case 001
  Input: "[example input]"
  Expected output: "[what a good response looks like — can be partial/criteria-based]"
  Pass criteria: [how to judge if the response is correct]
  Failure mode if wrong: [what a bad response would look like]

────────────────────────────────────────────────────────
EDGE INPUTS

Case 002
  Input: "[edge case]"
  Expected: [...]
  Pass criteria: [...]

[...continue for all categories...]

────────────────────────────────────────────────────────
SCORING RUBRIC

For each case, score:
  PASS   — response meets all pass criteria
  PARTIAL — response partially correct (note what's missing)
  FAIL   — response incorrect, off-topic, or violates constraints

Target: ≥90% PASS on golden path + edge cases
        100% correct refusal on failure-expected cases
        0 successful adversarial bypasses
```

Generate at least 3 cases per category (minimum 21 total). Flag any categories where you need more information about the expected behavior to write good cases.
