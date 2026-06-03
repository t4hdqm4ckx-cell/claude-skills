---
name: pr-description
description: Generate a clear PR title, summary, test plan, and reviewer notes from a git diff or change description
---

You are writing a pull request description.

**Step 1 — Get the change context**

Ask for (or extract from the user's message):
- The git diff, list of changed files, or plain-language description of what changed
- The motivation / why this change is being made (bug fix, feature, refactor, chore)
- Any linked issue or ticket number
- Target branch and any special merge considerations

**Step 2 — Analyze the change**

Before writing, classify the change:
- **feat**: new user-facing feature
- **fix**: bug fix
- **refactor**: restructuring without behavior change
- **perf**: performance improvement
- **test**: adding/updating tests
- **chore**: build, deps, CI, config — no production code change
- **docs**: documentation only

Identify:
- What is changing (the "what")
- Why it is changing (the "why" — this should dominate the description)
- What is NOT changing (scope boundaries — helps reviewers)
- Any risks or side effects

**Step 3 — Output format**

```
TITLE (under 72 chars):
[type]: [concise description of the change in imperative mood]

Examples:
  feat: add webhook delivery retry with exponential backoff
  fix: prevent double-charge when payment gateway times out
  refactor: extract auth middleware into standalone package

─────────────────────────────────────────────────────────────
PR BODY:

## What

[2-4 bullet points: the specific code changes made. Be concrete — name functions, files, or endpoints changed.]

- [Change 1]
- [Change 2]
- [Change 3]

## Why

[2-4 sentences on the motivation. Reference the bug, user pain, or technical debt being addressed. This is the most important section — reviewers need context, not just a list of changes.]

## How to test

- [ ] [Step 1: how to reproduce the scenario or invoke the feature]
- [ ] [Step 2: what to verify — exact behavior or output expected]
- [ ] [Step 3: edge case or regression check]
- [ ] [Automated tests added: yes/no — location if yes]

## Scope / what's NOT changing

[1-2 sentences on what is intentionally out of scope, to prevent scope creep in review comments.]

## Screenshots / output (if UI or CLI change)

[Placeholder — user should add before/after screenshot or terminal output]

## Related

- Closes #[issue]
- Related to #[issue] (if not closing)
- Depends on #[PR] (if stacked)

## Reviewer notes

[Optional: flag anything you're uncertain about, a decision you made that reviewers should weigh in on, or areas that need extra scrutiny.]
```

**Tone guidelines**
- Imperative mood in title ("add", "fix", "remove" — not "added", "fixing")
- Description is for reviewers who have zero context — don't assume they know the bug or feature
- Keep it scannable — bullets over paragraphs
- If the diff is large, call out the most important file/function to review first
