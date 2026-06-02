---
name: repo-init
description: Scaffold a new public GitHub repo with Kamil_K's standards — MIT License, INSTRUCTIONS.md, and HUMAN_IN_THE_LOOP.md (if agent/automation)
---

You are initializing a new public GitHub repository for Kamil_K. Follow these steps precisely:

**Step 1 — Gather info**
Ask the user (if not already provided in the command args):
- Repo name (kebab-case)
- One-line description
- Does this project involve an AI agent, automation, or decision-making pipeline? (Yes/No — determines if HUMAN_IN_THE_LOOP.md is needed)
- Preferred license (default: MIT)

**Step 2 — Create local directory**
```bash
mkdir -p ~/projects/<repo-name>
cd ~/projects/<repo-name>
git init
git config user.name "Kamil_K"
git config user.email "kamil7@yahoo.com"
```

**Step 3 — Create required root files**

Always create:
- `LICENSE` — MIT License with copyright "2026 Kamil_K"
- `README.md` — skeleton with project name, description, quick start, and structure sections
- `INSTRUCTIONS.md` — setup guide with prerequisites, install, run, and key files table
- `.gitignore` — appropriate for the project's language

If agent/automation involved, also create:
- `HUMAN_IN_THE_LOOP.md` — HITL policy with decision authority matrix, what the agent must never do autonomously, escalation protocol, and accountability roles

**Step 4 — Initial commits**
Commit each file separately with conventional commit messages:
- `chore: initial project scaffold with .gitignore`
- `chore: add MIT License`
- `docs: add README skeleton`
- `docs: add INSTRUCTIONS.md`
- `docs: add HUMAN_IN_THE_LOOP.md` (if applicable)

**Step 5 — Create GitHub repo**
```bash
gh repo create <repo-name> --public --description "<description>" --source=. --remote=origin
git push origin main
```

**Important standards to maintain:**
- Always use `Kamil_K` as git author and `kamil7@yahoo.com` as email
- Always use MIT license unless user specifies otherwise
- INSTRUCTIONS.md must include: prerequisites table, install steps, key files table, env vars table (if any), and troubleshooting section
- HUMAN_IN_THE_LOOP.md must include: decision authority matrix, 5 things the agent must never do, escalation SLAs, override procedures, and accountability table
- Commit messages must follow Conventional Commits (feat:/fix:/docs:/chore:/data:/test:)
