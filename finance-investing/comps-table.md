---
name: comps-table
description: Build a comparable company analysis table with EV/EBITDA, P/E, P/S, EV/Revenue multiples
---

You are building a comparable company (comps) analysis table.

**Step 1 — Get inputs**
Accept a list of companies from the user — either tickers or company names with financial data. The user may provide:
- A list of tickers (you'll ask them to supply the financials if no live data available)
- A table of financials they've pasted in
- A target company to value against the peer set

Minimum required per company: Revenue (TTM), EBITDA (TTM), Net Income (TTM), Market Cap, Net Debt, Shares Outstanding.

**Step 2 — Compute with Python**
```python
companies = [
    # {'name': 'Salesforce', 'market_cap': 220e9, 'net_debt': 3e9,
    #  'revenue': 34.9e9, 'ebitda': 8.2e9, 'net_income': 4.1e9,
    #  'revenue_growth': 0.11, 'ebitda_margin': 0.235}
]

for c in companies:
    ev = c['market_cap'] + c['net_debt']
    c['ev'] = ev
    c['ev_revenue']  = ev / c['revenue']      if c['revenue']   else None
    c['ev_ebitda']   = ev / c['ebitda']        if c['ebitda']    else None
    c['pe_ratio']    = c['market_cap'] / c['net_income']  if c.get('net_income') and c['net_income'] > 0 else None
    c['ps_ratio']    = c['market_cap'] / c['revenue']     if c['revenue']   else None

# Compute median multiples for valuation
import statistics
def median(vals):
    clean = [v for v in vals if v]
    return statistics.median(clean) if clean else None

med_ev_rev    = median([c['ev_revenue']  for c in companies])
med_ev_ebitda = median([c['ev_ebitda']   for c in companies])
med_pe        = median([c['pe_ratio']    for c in companies])
med_ps        = median([c['ps_ratio']    for c in companies])
```

**Step 3 — Present results**

```
COMPARABLE COMPANY ANALYSIS
────────────────────────────────────────────────────────────────────────
COMPANY        MktCap    EV      Rev    EBITDA%  EV/Rev  EV/EBITDA  P/E   P/S
─────────────  ──────    ──      ───    ───────  ──────  ─────────  ───   ───
Salesforce     $220B     $223B   $34.9B  23.5%   6.4x    27.2x     53.7x  6.3x
[Company 2]    ...
[Company 3]    ...

─────────────────────────────────────────────────────────────────────────
PEER MEDIAN                              X.Xx    XX.Xx     XX.Xx  X.Xx
PEER MEAN                                X.Xx    XX.Xx     XX.Xx  X.Xx

[If target company provided:]
TARGET COMPANY IMPLIED VALUATION (at peer median multiples)
  EV/Revenue  (Xr × $XB rev):    EV = $XX.XB
  EV/EBITDA   (Xr × $XB ebitda): EV = $XX.XB
  P/E         (Xr × $XB NI):     Equity = $XX.XB
  ──────────────────────────────────────────────
  Implied EV range:  $XX.XB – $XX.XB
  Implied per share: $XX – $XX  (XX% premium/discount to current)
```

Note: Flag outliers (multiples >2× the median) as potentially distorting the peer group.
Always state data vintage (TTM, LTM, NTM) and note that these are reference multiples only.
