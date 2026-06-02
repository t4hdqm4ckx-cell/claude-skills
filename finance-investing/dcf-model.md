---
name: dcf-model
description: Build a quick DCF valuation from revenue growth, margins, WACC, and terminal growth → intrinsic value per share
---

You are building a Discounted Cash Flow (DCF) model.

**Step 1 — Get inputs**
Ask the user for (or extract from their message):
- Company name / ticker
- Current revenue (TTM or last fiscal year)
- Revenue growth rate assumptions (Year 1–5, then terminal)
- EBIT or EBITDA margin (current and target)
- WACC (discount rate) — default 10% if not provided
- Terminal growth rate — default 3% if not provided
- Shares outstanding
- Net debt (debt minus cash) — for bridge to equity value
- Tax rate — default 25%
- Optional: capex as % of revenue (default 3%), D&A as % of revenue (default 2%)

**Step 2 — Build model with Python**
```python
def run_dcf(revenue, growth_rates, ebit_margins, wacc, terminal_growth,
            shares, net_debt, tax_rate=0.25, capex_pct=0.03, da_pct=0.02,
            projection_years=5):

    fcfs = []
    rev = revenue
    for i in range(projection_years):
        rev *= (1 + growth_rates[min(i, len(growth_rates)-1)])
        ebit = rev * ebit_margins[min(i, len(ebit_margins)-1)]
        nopat = ebit * (1 - tax_rate)
        da = rev * da_pct
        capex = rev * capex_pct
        fcf = nopat + da - capex
        fcfs.append((rev, ebit, fcf))

    # Terminal value (Gordon Growth)
    terminal_fcf = fcfs[-1][2] * (1 + terminal_growth)
    terminal_value = terminal_fcf / (wacc - terminal_growth)

    # Discount all cash flows
    pv_fcfs = [fcf / (1 + wacc)**(i+1) for i, (_, _, fcf) in enumerate(fcfs)]
    pv_terminal = terminal_value / (1 + wacc)**projection_years

    enterprise_value = sum(pv_fcfs) + pv_terminal
    equity_value = enterprise_value - net_debt
    intrinsic_per_share = equity_value / shares if shares else 0

    return {
        'projections': fcfs, 'pv_fcfs': pv_fcfs,
        'terminal_value': terminal_value, 'pv_terminal': pv_terminal,
        'enterprise_value': enterprise_value, 'equity_value': equity_value,
        'intrinsic_per_share': intrinsic_per_share,
        'tv_pct_of_ev': pv_terminal / enterprise_value * 100,
    }
```

**Step 3 — Present results**

```
DCF VALUATION — [Company] — [Date]
────────────────────────────────────────────────────────
ASSUMPTIONS
  WACC: X.X%  |  Terminal growth: X.X%  |  Tax rate: XX%
  Projection: 5 years

REVENUE & FCF PROJECTIONS ($M)
  Year   Revenue   Growth   EBIT%   FCF     PV(FCF)
  ────   ───────   ──────   ─────   ───     ───────
  Y1     $X,XXX    XX%      XX%     $XXX    $XXX
  ...
  Terminal Value:              $X,XXX  →  PV: $X,XXX (XX% of EV)

VALUATION SUMMARY
  Enterprise Value (EV):   $X,XXX M
  Less: Net Debt:          ($XXX M)
  Equity Value:            $X,XXX M
  Shares Outstanding:      XXX M
  ──────────────────────────────────
  Intrinsic Value / Share: $XX.XX

SENSITIVITY TABLE (Intrinsic Value per Share)
         WACC →    8%      9%     10%     11%     12%
  TGR ↓
  2%             $XX     $XX     $XX     $XX     $XX
  3%             $XX     $XX     $XX     $XX     $XX
  4%             $XX     $XX     $XX     $XX     $XX

NOTE: DCF is highly sensitive to terminal value assumptions (XX% of EV).
All projections are estimates — not financial advice.
```

Always include the sensitivity table. Always note the % of EV that is terminal value (high % = high uncertainty).
