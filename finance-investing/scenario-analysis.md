---
name: scenario-analysis
description: Stress test a P&L model under bear/base/bull assumptions with sensitivity tables
---

You are running a scenario analysis and stress test on a financial model.

**Step 1 — Get inputs**
Accept any of:
- A P&L model (revenue, costs, margins) with base-case assumptions
- Or: a SaaS metrics snapshot (MRR, churn, CAC, burn)
- The user may define custom scenarios or use the defaults below

Default scenarios:
- **Bear:** Revenue -20%, churn +2pp, CAC +30%, gross margin -5pp
- **Base:** No change from current trajectory
- **Bull:** Revenue +20%, churn -1pp, CAC -15%, gross margin +3pp

**Step 2 — Compute with Python**
```python
def apply_scenario(base, scenario_adjustments):
    result = dict(base)
    for key, adj in scenario_adjustments.items():
        if key in result:
            if isinstance(adj, dict):
                if adj['type'] == 'pct_change':
                    result[key] *= (1 + adj['value'])
                elif adj['type'] == 'additive':
                    result[key] += adj['value']
            else:
                result[key] = adj
    return result

def p_and_l(revenue, gross_margin, opex, interest_expense=0, tax_rate=0.25):
    gross_profit = revenue * gross_margin
    ebitda = gross_profit - opex
    ebit = ebitda  # simplified (no D&A separation)
    ebt = ebit - interest_expense
    net_income = ebt * (1 - tax_rate) if ebt > 0 else ebt
    fcf = ebitda * 0.85  # rough FCF conversion
    return {
        'revenue': revenue,
        'gross_profit': gross_profit,
        'gross_margin': gross_margin,
        'ebitda': ebitda,
        'ebitda_margin': ebitda / revenue if revenue else 0,
        'net_income': net_income,
        'fcf': fcf,
    }

scenarios = {
    'Bear':  {'revenue': 0.80, 'gross_margin': -0.05, 'opex': 1.05},
    'Base':  {'revenue': 1.00, 'gross_margin':  0.00, 'opex': 1.00},
    'Bull':  {'revenue': 1.20, 'gross_margin': +0.03, 'opex': 0.95},
}
```

**Step 3 — Present results**

```
SCENARIO ANALYSIS — [Company] — [Period]
────────────────────────────────────────────────────────────────
                      BEAR          BASE          BULL
Revenue:              $XXX (-20%)   $XXX          $XXX (+20%)
Gross Margin:         XX.X%         XX.X%         XX.X%
Gross Profit:         $XXX          $XXX          $XXX
EBITDA:               $XXX          $XXX          $XXX
EBITDA Margin:        XX.X%         XX.X%         XX.X%
Net Income:           $XXX          $XXX          $XXX
FCF:                  $XXX          $XXX          $XXX

KEY DELTAS (vs Base)
  Bear downside:  Revenue -$XXX | EBITDA -$XXX | FCF -$XXX
  Bull upside:    Revenue +$XXX | EBITDA +$XXX | FCF +$XXX

SENSITIVITY TABLE — EBITDA ($M) vs Revenue Growth & Margin
              Margin →   25%    30%    35%    40%
  Rev Growth ↓
  -20%                  $XX    $XX    $XX    $XX
   0%                   $XX    $XX    $XX    $XX
  +20%                  $XX    $XX    $XX    $XX

CASH IMPACT
  Bear scenario cash runway change: -X months
  Bull scenario cash runway change: +X months

RECOMMENDATION: [1-2 sentences on which scenario to plan for and why]
```

Always include the sensitivity table. Clearly label all scenario assumptions at the top.
If working with SaaS data, model churn changes as a direct impact on MRR/ARR, not just revenue growth.
