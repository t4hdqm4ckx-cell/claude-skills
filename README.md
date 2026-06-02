# claude-skills

Custom [Claude Code](https://claude.ai/code) skills organized by category. Each skill is a Markdown prompt file that Claude Code loads as a `/slash-command`.

## Install

Copy any skill to `~/.claude/skills/` and invoke it with `/skill-name` in Claude Code:

```bash
# Single skill
cp finance-investing/dcf-model.md ~/.claude/skills/

# Entire category
cp finance-investing/*.md ~/.claude/skills/

# Everything
cp **/*.md ~/.claude/skills/
```

See [INSTRUCTIONS.md](INSTRUCTIONS.md) for full setup details.

---

## Finance & Investing

| Skill | Description |
|---|---|
| [dcf-model](finance-investing/dcf-model.md) | DCF valuation from revenue growth, margins, WACC, and terminal growth → intrinsic value per share |
| [returns-calc](finance-investing/returns-calc.md) | Calculate total return, annualized return (CAGR), and dividend-adjusted return |
| [var-calc](finance-investing/var-calc.md) | Value at Risk — historical, parametric, and Monte Carlo approaches |
| [fx-exposure](finance-investing/fx-exposure.md) | Analyze FX currency exposure across a portfolio or business |
| [portfolio-snapshot](finance-investing/portfolio-snapshot.md) | Summarize portfolio allocation, concentration, and performance |
| [ratio-analysis](finance-investing/ratio-analysis.md) | Financial ratio breakdown (liquidity, leverage, profitability) with benchmarks |
| [rebalance-check](finance-investing/rebalance-check.md) | Detect portfolio drift from target allocation and suggest rebalancing trades |
| [comps-table](finance-investing/comps-table.md) | Build a comparable company analysis (comps) table |
| [scenario-analysis](finance-investing/scenario-analysis.md) | Model base, bull, and bear scenarios for a financial decision |
| [investment-memo](finance-investing/investment-memo.md) | Write a structured investment thesis memo |

---

## Business

| Skill | Description |
|---|---|
| [saas-summary](business/saas-summary.md) | SaaS metrics snapshot — ARR, MRR, churn, NRR, growth rate |
| [unit-economics](business/unit-economics.md) | LTV, CAC, payback period, and LTV:CAC ratio breakdown |
| [rule-of-40](business/rule-of-40.md) | Score a SaaS company on the Rule of 40 with commentary |
| [budget-vs-actual](business/budget-vs-actual.md) | Budget variance analysis — identify over/underspend by category |
| [cash-runway](business/cash-runway.md) | Burn rate, cash runway, and capital efficiency analysis |
| [earnings-brief](business/earnings-brief.md) | Earnings call summary — key metrics, guidance, and management tone |
| [renewal-check](business/renewal-check.md) | Vendor contract renewal review: pricing, usage, alternatives, negotiation levers. See also: [software-renewal-alert-agent](https://github.com/t4hdqm4ckx-cell/software-renewal-alert-agent) for automated contract monitoring. |

---

## Dev & Engineering

| Skill | Description |
|---|---|
| [debug](dev-engineering/debug.md) | Systematic debugging — reproduce, isolate, root-cause, fix |
| [refactor](dev-engineering/refactor.md) | Refactor code with explicit scope and safety guardrails |
| [git-flow](dev-engineering/git-flow.md) | Branch naming, commit messages, rebase vs merge, PR checklist |
| [sql-optimize](dev-engineering/sql-optimize.md) | Query optimization, index recommendations, and explain-plan analysis |
| [type-annotate](dev-engineering/type-annotate.md) | Add type annotations to untyped Python or TypeScript code |
| [test-gen](dev-engineering/test-gen.md) | Generate unit and integration test cases from existing code |
| [complexity](dev-engineering/complexity.md) | Cyclomatic complexity analysis with simplification suggestions |
| [perf-profile](dev-engineering/perf-profile.md) | Identify performance bottlenecks and propose targeted fixes |
| [regex-build](dev-engineering/regex-build.md) | Build, test, and explain regular expressions |
| [docker-help](dev-engineering/docker-help.md) | Dockerfile, Docker Compose, and container troubleshooting |
| [repo-init](dev-engineering/repo-init.md) | Scaffold a new repo with folder structure, README, and CI config |

---

## AI & Agents

| Skill | Description |
|---|---|
| [agent-design](ai-agents/agent-design.md) | Map out an agent workflow — tools, human checkpoints, loop structure, failure modes |
| [system-prompt](ai-agents/system-prompt.md) | Write and critique system prompts for clarity and reliability |
| [few-shot](ai-agents/few-shot.md) | Build few-shot example sets that improve model consistency |
| [eval-cases](ai-agents/eval-cases.md) | Generate evaluation test cases for AI systems and prompts |
| [prompt-improve](ai-agents/prompt-improve.md) | Critique and rewrite prompts for clarity, specificity, and output quality |
| [mcp-scaffold](ai-agents/mcp-scaffold.md) | Scaffold a new MCP server with tool definitions and handlers |
| [tool-schema](ai-agents/tool-schema.md) | Design Claude tool (function) schemas with well-described parameters |
| [context-trim](ai-agents/context-trim.md) | Identify and remove context window bloat to improve performance and cost |

---

## License

[MIT](LICENSE)
