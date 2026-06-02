# Instructions — claude-skills

A collection of custom Claude Code skills organized by category. Each skill is a Markdown file that Claude Code loads as a `/slash-command`.

## Prerequisites

- [Claude Code](https://claude.ai/code) CLI installed
- macOS or Linux

## Installation

Skills live in `~/.claude/skills/`. To install any skill from this repo, copy the `.md` file to that directory:

```bash
cp finance-investing/dcf-model.md ~/.claude/skills/
```

To install an entire category:

```bash
cp finance-investing/*.md ~/.claude/skills/
```

To install everything:

```bash
cp **/*.md ~/.claude/skills/
```

## Usage

Once a skill file is in `~/.claude/skills/`, invoke it in Claude Code by typing `/skill-name` in the prompt. The skill name matches the filename (without `.md`).

Example:

```
/dcf-model
/debug
/saas-summary
```

## File Structure

```
claude-skills/
├── finance-investing/   Valuation, portfolio, FX, and financial modeling skills
├── business/            SaaS metrics, unit economics, and business analysis skills
├── dev-engineering/     Debugging, refactoring, SQL, Git, and code quality skills
└── ai-agents/           Prompt engineering, agent design, and MCP tooling skills
```

## Updating Skills

Pull the latest and re-copy any updated files:

```bash
git pull
cp finance-investing/dcf-model.md ~/.claude/skills/
```

## Contributing

Fork the repo, add or improve skills, and open a PR. Skills should follow the frontmatter format:

```markdown
---
name: skill-name
description: One-line description of what the skill does
---

Skill prompt body...
```
