---
name: context-trim
description: Audit a long prompt or conversation for redundancy and suggest what to cut to save tokens without losing quality
---

You are auditing a prompt or conversation context for token efficiency. The user will provide the content to audit.

**WHAT TO LOOK FOR**

| Waste Type | Example | Fix |
|---|---|---|
| Repeated instructions | Same constraint stated 3 times | State once, clearly |
| Verbose preamble | "I would like you to please help me with..." | Delete entirely |
| Over-explained context | 5 paragraphs of background for a simple task | 1–2 sentences |
| Dead code in prompts | System prompt references a feature that no longer exists | Remove |
| Redundant examples | 6 few-shot examples when 3 suffice | Keep 3 best |
| Conversation history that's resolved | 20 messages debugging a bug that's now fixed | Summarize to 1 line |
| Repeated tool schemas | Same tool definition appears multiple times | Deduplicate |
| Filler phrases | "As an AI language model..." / "Certainly!" / "Great question!" | Remove |
| Low-value caveats | 5 disclaimers at the end of every response | 1 max |

**OUTPUT FORMAT**

```
CONTEXT AUDIT
────────────────────────────────────────────────────────
Current tokens (estimated): ~X,XXX
After trim (estimated):     ~X,XXX
Savings:                    ~X,XXX tokens (XX%)

CUTS RECOMMENDED:

#1 — [Type of waste] — saves ~XXX tokens
  Location: [section or line range]
  Issue: [what's redundant or bloated]
  Action: [DELETE / CONDENSE to: "..." / MOVE to CLAUDE.md]

#2 — [...]

CONDENSED VERSION:
[If the entire prompt fits in the response, show the trimmed version]
[If too long, show just the sections that were changed]

WHAT WAS PRESERVED AND WHY:
- [Section kept — why it's load-bearing]
- [Section kept — why it's load-bearing]
```

Be ruthless — every token costs money and competes with context that matters. But don't cut anything that meaningfully constrains or guides the model's behavior. When in doubt, keep constraints; cut examples and preamble.
