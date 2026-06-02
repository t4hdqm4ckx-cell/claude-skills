---
name: budget-vs-actual
description: Compare budget to actuals by department or category — variance flags, trend, and YTD summary
---

You are running a budget vs. actual analysis.

**Step 1 — Get data**
Look for data in this order:
1. `outputs/vendor_spend_report.xlsx` in the current or renewal-agent project directory
2. A CSV or table pasted by the user with columns: `category/department, budget, actual, period`
3. Direct inputs from the user

**Step 2 — Compute with Python**
```python
def safe_div(n, d): return n / d if d and d != 0 else 0

def analyze_variance(rows):
    # rows: list of {'dept': str, 'budget': float, 'actual': float, 'period': str}
    results = []
    for r in rows:
        var      = r['actual'] - r['budget']
        var_pct  = safe_div(var, r['budget'])
        severity = (
            'CRITICAL' if var_pct >  0.30 else
            'HIGH'     if var_pct >  0.20 else
            'MEDIUM'   if var_pct >  0.10 else
            'LOW'      if var_pct >  0.05 else
            'ON_TRACK' if var_pct >= -0.05 else
            'UNDER'    # under-spend
        )
        results.append({**r, 'variance': var, 'variance_pct': var_pct, 'severity': severity})
    return sorted(results, key=lambda x: abs(x['variance_pct']), reverse=True)

def ytd_summary(rows):
    total_budget = sum(r['budget'] for r in rows)
    total_actual = sum(r['actual'] for r in rows)
    return {
        'total_budget': total_budget,
        'total_actual': total_actual,
        'total_variance': total_actual - total_budget,
        'total_variance_pct': safe_div(total_actual - total_budget, total_budget),
        'over_budget_count': sum(1 for r in rows if r['actual'] > r['budget'] * 1.05),
        'under_budget_count': sum(1 for r in rows if r['actual'] < r['budget'] * 0.95),
    }
```

**Step 3 — Present results**

```
BUDGET VS. ACTUAL — [Period]
────────────────────────────────────────────────────────────────
YTD SUMMARY
  Total Budget:   $X,XXX,XXX
  Total Actual:   $X,XXX,XXX
  Net Variance:   +/-$XXX,XXX  (+/-X.X%)
  Over budget:    X departments / categories
  Under budget:   X departments / categories

DETAIL BY DEPARTMENT / CATEGORY
  Dept/Category     Budget      Actual      Variance    Var%    Status
  ─────────────     ──────      ──────      ────────    ────    ──────
  Engineering       $XXX,XXX    $XXX,XXX    +$XX,XXX    +XX%    🔴 CRITICAL
  Marketing         $XXX,XXX    $XXX,XXX    +$XX,XXX    +XX%    🟡 HIGH
  Cloud Infra       $XXX,XXX    $XXX,XXX    +$XX,XXX    +XX%    🟣 MEDIUM
  HR/People         $XXX,XXX    $XXX,XXX    -$XX,XXX    -XX%    🔵 UNDER
  ...
  ─────────────────────────────────────────────────────────────
  TOTAL             $X,XXX,XXX  $X,XXX,XXX  +/-$X,XXX   X.X%

TOP OVERSPEND ITEMS (if line-item data available)
  1. [Vendor/Item] — $X,XXX over ($X,XXX budget vs $X,XXX actual)
  2. ...

ACTIONS RECOMMENDED
  🔴 [Critical items]: Immediate review required — exceeds 30% over budget
  🟡 [High items]: Schedule review — exceeds 20% threshold
  💡 [Under-spend]: Verify under-spend is intentional (delayed hire, deferred project)
```

If loading from the renewal agent's `vendor_spend_report.xlsx`, use the Monthly Totals sheet for the summary and All Vendors sheet for line-item detail.
