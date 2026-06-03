---
name: ci-debug
description: Analyze CI/CD failure logs — identify root cause, classify failure type, and suggest a targeted fix
---

You are debugging a CI/CD pipeline failure.

**Step 1 — Get the failure context**

Ask for (or extract from the user's paste):
- CI platform (GitHub Actions, GitLab CI, CircleCI, Jenkins, etc.)
- The failing job/step name
- The raw log output (or relevant excerpt)
- What changed in the triggering commit/PR (optional but helpful)
- Whether this failure is new or recurring

**Step 2 — Classify the failure**

Identify which category this failure belongs to:

| Category | Signals |
|---|---|
| **Test failure** | assertion errors, `FAIL`, `expected X got Y`, test file paths |
| **Build/compile error** | syntax errors, missing imports, type errors, linker errors |
| **Dependency/env issue** | package not found, version mismatch, missing env var, network timeout |
| **Flaky test** | passes locally, intermittent, race condition signals, timing-related |
| **Infrastructure/runner** | OOM, disk full, runner disconnected, timeout unrelated to code |
| **Config/YAML error** | pipeline definition syntax, missing secret, wrong trigger |
| **Permissions/auth** | 401/403, token expired, missing IAM role, repo access denied |

**Step 3 — Output format**

```
CI DEBUG REPORT — [Job/Step Name]
─────────────────────────────────────────────────────────────
FAILURE CLASSIFICATION
  Type:     [Category from above]
  Severity: [Blocking / Flaky / Environment]
  New or recurring: [New / Recurring / Unknown]

ROOT CAUSE
  [2-4 sentences identifying the specific cause. Quote the key log line(s) that indicate it.]

  Key log signal:
  > [exact log line(s) that pinpoint the cause]

FIX
  [Step-by-step fix — be specific to the CI platform and language/tool involved]

  Quick command to verify locally (if applicable):
  $ [command]

IF FLAKY — mitigation options:
  □ Add retry logic: [platform-specific syntax]
  □ Add sleep/wait: [specific location]
  □ Fix race condition: [what to fix in test/code]

RELATED CHECKS
  □ [Other thing to verify — e.g., "check if env var is set in repo secrets"]
  □ [e.g., "confirm dependency version pinned in lockfile"]

─────────────────────────────────────────────────────────────
PREVENTION
[1-2 sentences on how to prevent this class of failure recurring — e.g., add the env var to a required secrets list, pin the dependency, add a retry decorator]
```

**If the log is too long:** Ask the user to share only the first error and the surrounding 20 lines — that's usually enough to diagnose.

**Common platform-specific tips to apply when relevant:**
- GitHub Actions: check `env:` context vs `secrets:` context; matrix job failures show only one axis
- Docker builds: layer cache invalidation often causes env/dep failures
- Node/Python: always check if lockfile is committed and used in install step
- Timeout failures: first check if it's a hanging test (no output for N min) vs slow network
