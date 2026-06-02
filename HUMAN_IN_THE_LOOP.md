# Human-in-the-Loop Policy

This repository contains AI skills (prompt templates) used with Claude Code. While the skills themselves are prompt files, some of them guide Claude to take actions that warrant human review before execution.

## Skills Requiring Human Approval Before Action

| Skill | Potential Action | Review Required |
|---|---|---|
| `repo-init` | Creates GitHub repos, pushes code | Confirm repo name, visibility, and remote before `git push` |
| `mcp-scaffold` | Scaffolds MCP server files | Review generated code before deploying |
| `agent-design` | Designs automation pipelines | Review tool access, write permissions, and loop conditions |
| `renewal-check` | May trigger contract/vendor actions | Confirm before sending any vendor communication |
| `git-flow` | Rewrites git history (rebase) | Confirm before force-push or history-destructive operations |

## General Principles

1. **Read-only is safe to auto-run.** Skills that only read, analyze, or summarize data (e.g., `dcf-model`, `ratio-analysis`, `debug`) require no special approval.

2. **Write actions need confirmation.** Any skill that would create files, push to remotes, send messages, or modify external systems should pause for user confirmation before proceeding.

3. **Financial outputs are advisory only.** Skills in `finance-investing/` and `business/` produce analytical outputs for decision support. They are not investment advice. All financial decisions remain with the human user.

4. **No autonomous execution.** These skills are invoked manually via `/skill-name` in Claude Code. There is no scheduled or automated execution of these skills without explicit user initiation.

## Escalation

If a skill produces unexpected output or proposes an action outside its stated scope, stop and ask the user to clarify before proceeding.

## Accountability

The human user invoking each skill is responsible for reviewing its output and approving any downstream actions.
