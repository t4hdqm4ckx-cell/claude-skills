---
name: system-prompt
description: Design an effective system prompt for a Claude-powered application from a brief description
---

You are designing a system prompt for a Claude-powered application. The user will describe what they're building. Generate a production-quality system prompt.

**SYSTEM PROMPT ANATOMY**

A good system prompt has these sections in this order:
1. **Role** — what Claude is and who it serves
2. **Context** — the product, the user base, the stakes
3. **Core behavior** — what Claude should always do
4. **Constraints** — what Claude must never do
5. **Output format** — default response format, length, tone
6. **Edge cases** — how to handle off-topic requests, uncertainty, sensitive topics

**PRINCIPLES**

- Be specific about the role — "You are a contract renewal assistant for a startup's Finance team" beats "You are a helpful assistant"
- State the negative space — what the assistant should NOT do is as important as what it should
- Define the audience — "users are non-technical finance managers" changes vocabulary and explanation depth
- Set a default response length — short for chat, longer for document generation
- Handle uncertainty explicitly — "If you don't know, say so and suggest who to ask"
- Don't over-constrain — every rule you add is a rule Claude has to follow even in edge cases; add rules only for real risks

**OUTPUT**

Generate the full system prompt ready to use, followed by:

```
SYSTEM PROMPT:
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[Complete system prompt here]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DESIGN NOTES:
- Role choice: [why you framed the role this way]
- Key constraints: [the most important do-nots and why]
- What's NOT in here: [anything deliberately omitted and why]

CLAUDE.md PLACEMENT:
[Copy the above into your project's CLAUDE.md if this is a code project,
or pass as the `system` parameter in the Anthropic API]

API USAGE:
  import anthropic
  client = anthropic.Anthropic()
  response = client.messages.create(
      model="claude-sonnet-4-6",
      max_tokens=1024,
      system="[YOUR SYSTEM PROMPT]",
      messages=[{"role": "user", "content": user_input}]
  )

SUGGESTED TESTS:
1. [Test input that should work well]
2. [Edge case the system prompt should handle]
3. [Off-topic request to verify the constraint works]
```
