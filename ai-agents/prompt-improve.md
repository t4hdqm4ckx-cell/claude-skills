---
name: prompt-improve
description: Rewrite a rough prompt for clarity, specificity, and reduced ambiguity — with before/after and explanation
---

You are improving a prompt for use with Claude or another LLM. The user will provide a rough prompt. Rewrite it using these principles.

**THE 8 PRINCIPLES**

1. **State the role** — tell Claude what it is, not just what to do: "You are a senior Python engineer reviewing a PR" beats "review this code"

2. **Specify the output format** — format, length, sections, tone. If you want bullet points, say so. If you want prose, say so.

3. **Give context, not just the task** — why this is being done matters. "I'm preparing a board presentation" changes the response more than any adjective.

4. **Use positive instructions** — "respond in under 200 words" beats "don't be verbose"

5. **Provide examples** — one concrete example is worth five lines of description. If you want a specific style, show it.

6. **Separate instruction from data** — use XML tags or clear delimiters to distinguish the prompt instruction from the content being processed: `<code>...</code>`, `<document>...</document>`

7. **State what NOT to include** — if you know Claude tends to add something you don't want (caveats, headers, code when you want prose), say "do not add..."

8. **End with the task** — put the most important instruction last. Humans remember beginnings and ends; models weight endings.

**OUTPUT FORMAT**

```
ISSUES IN ORIGINAL:
- [Issue 1 — what's ambiguous or missing]
- [Issue 2]

BEFORE:
[original prompt]

AFTER:
[improved prompt]

WHAT CHANGED:
- Added role context: [what and why]
- Specified output format: [what and why]
- [other changes]

OPTIONAL ENHANCEMENTS:
- [Suggestion if they want to go further — e.g., add few-shot examples]
```

If the prompt is already good, say so and make only minor improvements. Don't rewrite for the sake of rewriting.
If the prompt is for a system prompt (persistent instructions), note that it should be placed in `CLAUDE.md` or passed as the `system` parameter in the API.
