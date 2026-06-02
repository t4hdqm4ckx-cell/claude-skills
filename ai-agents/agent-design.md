---
name: agent-design
description: Map out an agent workflow — tools needed, human review checkpoints, loop structure, failure modes
---

You are designing an AI agent architecture. The user will describe what the agent should do. Produce a complete design document.

**DESIGN QUESTIONS TO ANSWER**

Before generating the design, clarify (or state your assumptions if obvious):
1. What is the agent's primary goal? (one sentence)
2. What data sources does it read? What systems does it write to?
3. What decisions can it make autonomously vs. what requires human approval?
4. How often does it run? (one-shot, scheduled, event-triggered, always-on)
5. What does failure look like, and how should it be handled?

**OUTPUT FORMAT**

```
AGENT DESIGN — [Agent Name]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PURPOSE
  [One sentence: what the agent does and who benefits]

TRIGGER
  [How the agent starts: cron schedule / webhook / user command / event]

INPUTS
  - [Data source 1 — type, location, format]
  - [Data source 2]

OUTPUTS
  - [Output 1 — what is produced, where it goes]
  - [Output 2]

TOOL INVENTORY
  Tool name          | Description                          | Autonomous?
  ─────────────────  | ────────────────────────────────────  | ──────────
  read_contracts()   | Load contract data from Excel        | ✅ Yes
  evaluate_alerts()  | Run alert engine against contracts   | ✅ Yes
  send_notification()| Post to Slack / send email           | ❌ Human required
  cancel_contract()  | Initiate cancellation workflow       | ❌ Human required

AGENT LOOP (pseudocode)
  1. [Step 1 — describe what happens]
  2. [Step 2]
  3. [HUMAN CHECKPOINT — what decision requires review]
  4. [Step 4 — continues after human approves]
  ...

HUMAN-IN-THE-LOOP GATES
  Gate 1: [What triggers it, who reviews, what they decide, max wait time]
  Gate 2: [...]

FAILURE MODES & HANDLING
  Failure: [What can go wrong]
  Detection: [How the agent knows it failed]
  Response: [What the agent does — retry / alert human / halt]

  Failure: [...]

STATE MANAGEMENT
  [How does the agent track what it's done? File, DB, memory? What happens on restart?]

MODULES TO BUILD
  - [module_name.py] — [what it does]
  - [module_name.py] — [what it does]
  - [tests/test_*.py] — [what to test]

SECURITY CONSIDERATIONS
  - [Credential handling]
  - [Data that must not be logged]
  - [Rate limits or blast radius controls]

OPEN QUESTIONS
  - [Anything that needs more information before building]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

After the design, recommend which modules to build first (the critical path) and what the first working version (MVP) would look like.
