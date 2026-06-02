---
name: few-shot
description: Generate 3–5 high-quality few-shot examples for a task to include in a prompt
---

You are generating few-shot examples to improve a prompt. The user will describe the task and the desired output format. Generate diverse, high-quality input/output pairs.

**WHAT MAKES A GOOD FEW-SHOT EXAMPLE**

1. **Representative** — covers the typical case the prompt will encounter
2. **Diverse** — examples differ meaningfully; do not repeat the same pattern with different words
3. **Correct** — the output is exactly what you want, including format, tone, and length
4. **Ordered** — put easiest first, hardest last; models weight the final example most heavily
5. **Edge-covering** — include at least one non-obvious case that teaches the model where the boundaries are

**OUTPUT FORMAT**

Produce examples in the exact format the user wants them injected into the prompt. Default to XML tags:

```
FEW-SHOT EXAMPLES — [Task name]
[N] examples

USAGE: Insert these between your instructions and the live input in your prompt.

<examples>

<example>
<input>
[input text here]
</input>
<output>
[ideal output here — format exactly as you want the model to respond]
</output>
</example>

<example>
<input>
[different input — vary the topic, length, or complexity]
</input>
<output>
[ideal output]
</output>
</example>

<example>
<input>
[edge case or non-obvious scenario]
</input>
<output>
[shows the model how to handle the boundary]
</output>
</example>

</examples>
```

After the examples:
```
DIVERSITY CHECK:
  Example 1: [what dimension it covers]
  Example 2: [what dimension it covers]
  Example 3: [what dimension it covers]

WHAT THESE TEACH THE MODEL:
  - [Implicit rule example 1 demonstrates]
  - [Implicit rule example 2 demonstrates]
  - [Boundary case example 3 demonstrates]

SUGGESTED PLACEMENT IN PROMPT:
  [instructions]
  [examples block]
  [live input: {{USER_INPUT}}]
```

Generate at least 3 examples, up to 5. More than 5 rarely improves performance and costs tokens.
Ask the user for 1–2 real examples from their use case if available — real examples always outperform synthetic ones.
