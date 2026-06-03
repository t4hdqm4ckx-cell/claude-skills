---
name: macro-brief
description: Produce a macro environment snapshot — rates, inflation, growth, USD, risk-on/off sentiment, and implications for asset allocation
---

You are producing a structured macro environment brief.

**Step 1 — Scope**

Ask the user:
1. Date/period of focus (default: current)
2. Region focus: Global, US, Europe, EM, or specific country?
3. Purpose: investment positioning, business planning, or general awareness?

If the user provides data or headlines, use them. Otherwise, work from your training knowledge and clearly state the knowledge cutoff.

**Step 2 — Framework**

Cover these macro dimensions:

1. **Monetary policy** — Central bank stance (hiking/holding/cutting), current policy rate, forward guidance, QT/QE status
2. **Inflation** — CPI/PCE level and trend, core vs headline, drivers (energy, shelter, services)
3. **Growth** — GDP trend, leading indicators (PMI, jobless claims), recession probability
4. **Labor market** — Unemployment rate, wage growth, participation rate
5. **Credit conditions** — Spreads (IG, HY), lending standards, credit stress signals
6. **Currency (USD)** — DXY trend, key cross-rates, impact on EM and commodities
7. **Risk sentiment** — VIX level, equity positioning, safe-haven flows
8. **Geopolitical / structural** — Any dominant macro tail risks

**Step 3 — Output format**

```
MACRO BRIEF — [Region] | [Period]
─────────────────────────────────────────────────────────────
REGIME SUMMARY
  Overall: [Risk-On / Risk-Off / Transitional]
  Cycle stage: [Early / Mid / Late / Recession]

MONETARY POLICY        [Hawkish / Neutral / Dovish]
  [2-3 sentences on central bank stance and trajectory]

INFLATION              [Accelerating / Stable / Decelerating]
  [2-3 sentences on CPI/PCE, trend, key drivers]

GROWTH                 [Expanding / Slowing / Contracting]
  [2-3 sentences on GDP, PMIs, recession signals]

LABOR MARKET           [Tight / Easing / Loose]
  [1-2 sentences]

CREDIT CONDITIONS      [Easy / Tightening / Stressed]
  [1-2 sentences on spreads and lending]

USD                    [Strengthening / Stable / Weakening]
  [1-2 sentences on DXY and cross-rate implications]

RISK SENTIMENT         [Elevated risk appetite / Cautious / Risk-off]
  [1-2 sentences on VIX, positioning]

TAIL RISKS
  1. [Risk and potential market impact]
  2. [Risk and potential market impact]

─────────────────────────────────────────────────────────────
ASSET ALLOCATION IMPLICATIONS
  Equities:      [Favorable / Neutral / Cautious] — [1-line rationale]
  Fixed income:  [Favorable / Neutral / Cautious] — [1-line rationale]
  Commodities:   [Favorable / Neutral / Cautious] — [1-line rationale]
  Cash/USD:      [Favorable / Neutral / Cautious] — [1-line rationale]
  EM assets:     [Favorable / Neutral / Cautious] — [1-line rationale]

DATA FRESHNESS
  Based on: [user-provided data / training knowledge as of [date]]
  Recommend verifying: [key figures the user should cross-check]
```

**Always remind the user** this is a macro summary for analytical context, not investment advice.
