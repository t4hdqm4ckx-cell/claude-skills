---
name: rule-of-40
description: SaaS Rule of 40 calculator — revenue growth + profit margin, with benchmarks and trend analysis
---

You are computing the SaaS Rule of 40 score and related efficiency metrics.

**Step 1 — Get inputs**
Accept any of:
- Revenue figures for 2+ periods (to compute growth)
- Or: direct revenue growth % + profit margin %
- Profit metric: EBITDA margin, operating margin, FCF margin, or net margin (note which is used)
- Optional: multiple periods for trend analysis

**Step 2 — Compute with Python**
```python
def rule_of_40(revenue_growth_pct, profit_margin_pct):
    score = revenue_growth_pct + profit_margin_pct
    if score >= 60:   grade = "Exceptional (top-tier SaaS)"
    elif score >= 40: grade = "Healthy (Rule of 40 achieved)"
    elif score >= 25: grade = "Developing (approaching benchmark)"
    else:             grade = "Needs improvement"
    return score, grade

def magic_number(new_arr, prev_arr, s_and_m_spend):
    # Measures sales efficiency: how much ARR per $ of S&M
    return (new_arr - prev_arr) / s_and_m_spend if s_and_m_spend else None

def burn_multiple(net_burn, net_new_arr):
    # How much cash burned per $ of net new ARR
    return abs(net_burn) / net_new_arr if net_new_arr else None

def arr_per_fte(arr, headcount):
    return arr / headcount if headcount else None
```

**Step 3 — Present results**

```
RULE OF 40 ANALYSIS — [Company] — [Period]
────────────────────────────────────────────
Revenue Growth:    +XX.X%
[FCF/EBITDA/Op] Margin: XX.X%
                   ─────────
Rule of 40 Score:  XX  → [Grade]

[If multiple periods:]
TREND
  Period      Rev Growth   Margin   R40 Score
  ──────      ──────────   ──────   ─────────
  Q1 2025     +XX%         XX%      XX
  Q2 2025     +XX%         XX%      XX
  ...

[If additional metrics provided:]
EFFICIENCY METRICS
  Magic Number:    X.XX  (>0.75 = efficient, >1.0 = excellent)
  Burn Multiple:   X.Xx  (<1.5 = efficient, <1.0 = exceptional)
  ARR per FTE:     $XXX K

BENCHMARKS (public SaaS, 2025)
  Top quartile:    R40 > 60
  Median:          R40 ~35–40
  Bottom quartile: R40 < 20
  Your score:      XX  (Xth percentile estimated)
```

Note which profit margin metric is being used — FCF margin is preferred by investors for growth-stage SaaS; EBITDA is more common for mature companies.
