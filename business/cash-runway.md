---
name: cash-runway
description: Calculate cash runway in months, zero-cash date, and break-even from current cash, burn rate, and revenue
---

You are computing cash runway and burn analysis.

**Step 1 — Get inputs**
Accept any of:
- Direct: current cash balance, monthly net burn, monthly revenue (optional)
- Or: cash balance + monthly expense breakdown + monthly revenue
- Optional: projected revenue growth rate (to model dynamic runway)
- Optional: planned fundraise amount and expected close date

**Step 2 — Compute with Python**
```python
from datetime import date, timedelta

def months_to_date(start_date, months):
    m = start_date.month - 1 + months
    year = start_date.year + m // 12
    month = m % 12 + 1
    return start_date.replace(year=year, month=month, day=1)

def runway(cash, monthly_burn, monthly_revenue=0,
           revenue_growth_monthly=0, reference_date=None):
    ref = reference_date or date(2026, 6, 1)
    net_burn = monthly_burn - monthly_revenue  # net cash consumed
    
    if net_burn <= 0:
        return {'status': 'cash_flow_positive', 'months': float('inf')}
    
    # Static runway (no growth)
    static_months = cash / net_burn
    zero_cash_date = months_to_date(ref, int(static_months))
    
    # Dynamic runway (with revenue growth)
    balance, month = cash, 0
    while balance > 0 and month < 120:
        rev = monthly_revenue * (1 + revenue_growth_monthly) ** month
        net = monthly_burn - rev
        balance -= net
        month += 1
        if net <= 0:
            return {'status': 'break_even_reached', 'months_to_breakeven': month,
                    'breakeven_revenue': rev}
    
    return {
        'status': 'burning',
        'static_runway_months': round(static_months, 1),
        'zero_cash_date': zero_cash_date.strftime('%b %Y'),
        'dynamic_runway_months': month,
        'monthly_net_burn': net_burn,
        'annual_burn_rate': net_burn * 12,
    }

def burn_efficiency(new_arr, net_burn_monthly):
    # Burn multiple: $ burned per $ of net new ARR
    return abs(net_burn_monthly * 12) / new_arr if new_arr else None
```

**Step 3 — Present results**

```
CASH RUNWAY ANALYSIS — [Company] — [Date]
────────────────────────────────────────────────────────
BURN SUMMARY
  Current Cash:           $X,XXX,XXX
  Monthly Gross Burn:     ($XXX,XXX)
  Monthly Revenue:        +$XXX,XXX
  Monthly Net Burn:       ($XXX,XXX)
  Annual Burn Rate:       ($X,XXX,XXX)

RUNWAY
  Static Runway:          XX.X months  → Zero cash: [Mon YYYY]
  Dynamic Runway*:        XX.X months  → Zero cash: [Mon YYYY]
  (*with X% monthly revenue growth)

MILESTONES
  ⚠  18-month warning:   [Mon YYYY]  ← Begin fundraise by this date
  🔴 12-month critical:  [Mon YYYY]  ← Must have term sheet
  Zero cash:             [Mon YYYY]

BREAK-EVEN ANALYSIS
  Current monthly revenue needed to break even: $XXX,XXX
  Gap to break-even: $XXX,XXX/mo  (XX% of current revenue)
  Months to break-even at X% growth: ~XX months

[If fundraise planned:]
  POST-RAISE RUNWAY: $X,XXX,XXX additional → +XX months
  Total projected runway: XX months → [Mon YYYY]

RECOMMENDATION: [1-2 sentences on urgency and recommended action]
```

Flag CRITICAL if runway < 12 months. Flag WARNING if runway < 18 months.
Note that fundraising typically takes 3–6 months — factor this into urgency.
