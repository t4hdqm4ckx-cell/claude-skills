---
name: portfolio-snapshot
description: Given a CSV of holdings, compute total value, allocation %, unrealized P&L, and top/bottom performers
---

You are generating a portfolio snapshot. Follow these steps:

**Step 1 — Get the data**
Look for a holdings CSV in the current directory or ask the user to provide one. Expected columns (flexible):
`ticker, shares, avg_cost, current_price` (or similar). If no file exists, ask the user to paste holdings as a table or provide the path.

**Step 2 — Compute with Python**
```python
import csv, os

# Load holdings — adapt column names to what's in the file
holdings = []
# Example structure after loading:
# {'ticker': 'AAPL', 'shares': 10, 'avg_cost': 150.0, 'current_price': 185.0}

for h in holdings:
    h['market_value'] = h['shares'] * h['current_price']
    h['cost_basis']   = h['shares'] * h['avg_cost']
    h['unrealized_pl'] = h['market_value'] - h['cost_basis']
    h['pl_pct']        = (h['unrealized_pl'] / h['cost_basis']) * 100 if h['cost_basis'] else 0

total_value    = sum(h['market_value'] for h in holdings)
total_cost     = sum(h['cost_basis'] for h in holdings)
total_pl       = total_value - total_cost
total_pl_pct   = (total_pl / total_cost) * 100 if total_cost else 0

for h in holdings:
    h['allocation_pct'] = (h['market_value'] / total_value) * 100 if total_value else 0
```

**Step 3 — Present results**

Format the output as:

```
PORTFOLIO SNAPSHOT — [date]
────────────────────────────────────────
Total Value:    $X,XXX,XXX
Total Cost:     $X,XXX,XXX
Unrealized P&L: $+/-X,XXX (+/-X.X%)

ALLOCATION
  TICKER   Shares   Price    Value       Alloc%   P&L
  ──────   ──────   ─────    ─────       ──────   ───
  AAPL     10       $185     $1,850      12.3%    +$350 (+23.3%)
  ...

TOP 3 PERFORMERS:   TICKER (+X.X%) ...
BOTTOM 3 PERFORMERS: TICKER (-X.X%) ...

CONCENTRATION: Top 5 holdings = X.X% of portfolio
```

If current prices aren't provided, note that and compute cost-basis allocation only.
Flag any single position >20% of portfolio as a concentration risk.
