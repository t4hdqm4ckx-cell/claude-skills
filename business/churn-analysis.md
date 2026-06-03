---
name: churn-analysis
description: Cohort churn breakdown — monthly/annual churn rates, retention curves, revenue churn vs logo churn, and root cause hypotheses
---

You are analyzing customer churn for a SaaS or subscription business.

**Step 1 — Gather inputs**

Ask for (or extract from the user's message):
- Cohort data: customers acquired per month/quarter and when they churned
- OR: aggregate monthly active customers and churned counts
- Revenue data if available (for revenue churn vs logo churn split)
- Customer segments if relevant (plan tier, company size, channel)
- Any known events (pricing change, product launch, support issues)

**Step 2 — Calculate**

Key metrics to derive:

- **Logo churn rate** (monthly): Churned customers / Customers at start of period
- **Revenue churn rate**: Churned MRR / MRR at start of period
- **Net Revenue Retention (NRR)**: (Starting MRR + Expansion - Contraction - Churn) / Starting MRR
- **Gross Revenue Retention (GRR)**: (Starting MRR - Contraction - Churn) / Starting MRR
- **Retention curve by cohort**: % of each cohort still active at month 1, 3, 6, 12, 24
- **Average customer lifetime**: 1 / monthly churn rate
- **LTV impact**: show how reducing churn by 1pp changes LTV

**Step 3 — Output format**

```
CHURN ANALYSIS — [Company/Product] | [Period]
─────────────────────────────────────────────────────────────
HEADLINE METRICS
  Monthly logo churn:    [X]%   (annual equiv: [X]%)
  Monthly revenue churn: [X]%
  Net Revenue Retention: [X]%   [benchmark: healthy SaaS >100%]
  Gross Revenue Retention:[X]%  [benchmark: healthy SaaS >85%]
  Avg customer lifetime: [X] months

RETENTION CURVE (% of cohort still active)
  Month 1:   [X]%
  Month 3:   [X]%
  Month 6:   [X]%
  Month 12:  [X]%
  Month 24:  [X]%  [if data available]

  Pattern: [Quick drop + plateau / Steady decline / Delayed cliff]

COHORT COMPARISON (if multi-cohort data)
  Best cohort:  [Month/Year] — [X]% retained at 12 months
  Worst cohort: [Month/Year] — [X]% retained at 12 months
  Trend:        [Improving / Stable / Deteriorating]

CHURN BY SEGMENT (if data available)
  [Segment]    [Churn rate]    [vs average]    [ARR at risk]
  ─────────────────────────────────────────────────────────

SENSITIVITY: 1pp CHURN REDUCTION IMPACT
  LTV increase:      +$[X] per customer
  ARR impact (12mo): +$[X]

─────────────────────────────────────────────────────────────
ROOT CAUSE HYPOTHESES
Based on the patterns above, likely drivers:
1. [Hypothesis + supporting evidence from data]
2. [Hypothesis + supporting evidence from data]
3. [Hypothesis + supporting evidence from data]

RECOMMENDED ACTIONS
1. [Specific action targeting highest-impact churn driver]
2. [Specific action]
3. [Specific action]

DATA GAPS
[What additional data would sharpen this analysis]
```

**Ask follow-up if needed:** Do you have exit survey data or support ticket themes that could explain the churn drivers?
