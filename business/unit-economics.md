---
name: unit-economics
description: Compute LTV, CAC, LTV:CAC ratio, payback period, and gross margin from SaaS inputs
---

You are computing SaaS unit economics. These are the fundamental metrics for evaluating business quality and growth sustainability.

**Step 1 — Get inputs**
Accept any of:
- Direct inputs: ARPU, churn rate, gross margin, CAC
- Or: MRR/ARR, customer count, new customers, S&M spend, COGS
- Optional: data file path (JSON or Excel from saas-metrics-agent or similar)

**Step 2 — Compute with Python**
```python
def safe_div(n, d, fallback=0): return n / d if d and d != 0 else fallback

def unit_economics(arpu_monthly, churn_rate_monthly, gross_margin,
                   cac, expansion_rate_monthly=0):
    # LTV = ARPU * gross_margin / (churn - expansion)
    net_churn = churn_rate_monthly - expansion_rate_monthly
    ltv = safe_div(arpu_monthly * gross_margin, max(net_churn, 0.001))
    ltv_cac = safe_div(ltv, cac)
    payback_months = safe_div(cac, arpu_monthly * gross_margin)
    customer_lifetime_months = safe_div(1, churn_rate_monthly)
    
    return {
        'ltv': ltv,
        'cac': cac,
        'ltv_cac': ltv_cac,
        'payback_months': payback_months,
        'customer_lifetime_months': customer_lifetime_months,
        'annual_revenue_per_customer': arpu_monthly * 12,
        'gross_margin': gross_margin,
    }

def benchmark(ltv_cac, payback_months):
    ltv_grade = (
        "Exceptional (>5x)" if ltv_cac > 5 else
        "Healthy (3–5x)"    if ltv_cac >= 3 else
        "Below target (<3x)"
    )
    payback_grade = (
        "Excellent (<12mo)"    if payback_months < 12 else
        "Good (12–18mo)"       if payback_months <= 18 else
        "Needs improvement (>18mo)"
    )
    return ltv_grade, payback_grade
```

**Step 3 — Present results**

```
UNIT ECONOMICS — [Company] — [Period]
──────────────────────────────────────────────────────────
INPUTS
  ARPU (monthly):        $XXX
  Gross Margin:          XX.X%
  Monthly Churn:         X.XX%  → Annual: XX.X%
  CAC:                   $X,XXX

CORE METRICS
  Customer Lifetime:     XX months (X.X years)
  LTV:                   $X,XXX   → [Grade]
  CAC:                   $X,XXX
  LTV:CAC Ratio:         X.Xx     → [Grade]
  CAC Payback Period:    XX months → [Grade]

UNIT P&L (per customer, lifetime)
  Revenue:               $X,XXX
  Gross Profit:          $X,XXX  (XX%)
  Less: CAC:             ($X,XXX)
  Net Unit Contribution: $X,XXX  (X.Xx multiple)

BENCHMARKS
  LTV:CAC > 3x = Venture-backable
  LTV:CAC > 5x = Exceptional
  Payback < 12 months = Capital efficient
  Payback < 18 months = Acceptable for growth-stage

[If FlowSync/renewal agent data available, pull actuals automatically]
```

If the user is in the saas-metrics-agent project, attempt to load `data/synthetic_data.json` and compute actuals automatically.
