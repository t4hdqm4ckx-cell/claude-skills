---
name: ltv-cac
description: Deep-dive LTV:CAC analysis — lifetime value, acquisition cost, payback period, efficiency benchmarks, and improvement levers
---

You are performing a detailed LTV:CAC analysis for a subscription or SaaS business.

**Step 1 — Gather inputs**

Ask for (or extract):
- Average Revenue Per Account (ARPA) — monthly or annual
- Gross margin (%) — blended across product
- Monthly or annual churn rate
- Customer Acquisition Cost (CAC) — total sales & marketing spend / new customers acquired in period
- Sales & marketing spend breakdown if available (paid, organic, sales team, etc.)
- Customer segments if relevant (SMB, Mid-Market, Enterprise)
- Expansion revenue / upsell rate if applicable

**Step 2 — Calculate**

```
LTV  = (ARPA × Gross Margin %) / Monthly Churn Rate
     = ARPA × Gross Margin % × Average Customer Lifetime

CAC Payback Period = CAC / (ARPA × Gross Margin %)   [in months]

LTV:CAC Ratio = LTV / CAC
```

For expansion-adjusted LTV:
```
LTV (with expansion) = (ARPA × GM%) / (Churn Rate - Expansion Rate)
```

**Step 3 — Output format**

```
LTV:CAC ANALYSIS — [Company/Product] | [Period]
─────────────────────────────────────────────────────────────
INPUTS
  ARPA (monthly):   $[X]      Gross margin:  [X]%
  Monthly churn:    [X]%      Monthly expansion: [X]% (if any)
  CAC:              $[X]      Avg lifetime: [X] months

CORE METRICS
  LTV (gross margin):        $[X]
  LTV (expansion-adjusted):  $[X]   [if expansion data provided]
  LTV:CAC ratio:             [X]:1

  CAC payback period:        [X] months

BENCHMARK COMPARISON
  Metric            Your value    Healthy SaaS    Best-in-class
  ──────────────    ──────────    ────────────    ─────────────
  LTV:CAC           [X]:1         3:1             5:1+
  CAC payback       [X] mo        <12 mo          <6 mo
  NRR (implied)     [X]%          >100%           >120%

VERDICT
  [One sentence: is the business efficient, at risk, or world-class? What's the single most important lever?]

─────────────────────────────────────────────────────────────
SENSITIVITY ANALYSIS

  Impact of reducing churn by 1pp:
    LTV → $[X] (+[X]%)    |    LTV:CAC → [X]:1

  Impact of reducing CAC by 20%:
    Payback → [X] months  |    LTV:CAC → [X]:1

  Impact of 10% ARPA increase (pricing/upsell):
    LTV → $[X]            |    Payback → [X] months

IMPROVEMENT LEVERS (ranked by impact)
1. [Lever — e.g., reduce churn] → [specific action] → est. [X]% LTV improvement
2. [Lever] → [action] → est. impact
3. [Lever] → [action] → est. impact

BY SEGMENT (if data provided)
  Segment     ARPA    Churn    CAC     LTV:CAC    Payback
  ────────    ────    ─────    ───     ───────    ───────
  [SMB]       $[X]    [X]%     $[X]    [X]:1      [X] mo
  [MM]        $[X]    [X]%     $[X]    [X]:1      [X] mo
  [ENT]       $[X]    [X]%     $[X]    [X]:1      [X] mo
```

Always flag if LTV:CAC < 1 (destroying value) or payback > 24 months (dangerous for capital efficiency).
