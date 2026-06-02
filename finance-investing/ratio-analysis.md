---
name: ratio-analysis
description: Compute 15 standard financial ratios — margins, liquidity, leverage, efficiency — from raw financials
---

You are computing a full financial ratio analysis from raw financial statement inputs.

**Step 1 — Get inputs**
Ask the user to provide (or extract from pasted text):

Income Statement: Revenue, COGS, Gross Profit, EBITDA, EBIT, Net Income, Interest Expense, Tax Expense
Balance Sheet: Current Assets, Current Liabilities, Cash, Inventory, Total Assets, Total Equity, Total Debt
Cash Flow: Operating CF, Capex, Free Cash Flow

**Step 2 — Compute with Python**
```python
def safe_div(n, d): return n / d if d and d != 0 else None

def compute_ratios(r):
    # r = dict with all financials
    return {
        # Profitability
        'gross_margin':    safe_div(r.get('gross_profit'),  r.get('revenue')),
        'ebitda_margin':   safe_div(r.get('ebitda'),        r.get('revenue')),
        'ebit_margin':     safe_div(r.get('ebit'),          r.get('revenue')),
        'net_margin':      safe_div(r.get('net_income'),    r.get('revenue')),
        'fcf_margin':      safe_div(r.get('fcf'),           r.get('revenue')),
        'roe':             safe_div(r.get('net_income'),    r.get('total_equity')),
        'roa':             safe_div(r.get('net_income'),    r.get('total_assets')),

        # Liquidity
        'current_ratio':   safe_div(r.get('current_assets'),  r.get('current_liabilities')),
        'quick_ratio':     safe_div(r.get('current_assets') - r.get('inventory', 0), r.get('current_liabilities')),
        'cash_ratio':      safe_div(r.get('cash'),          r.get('current_liabilities')),

        # Leverage
        'debt_to_equity':  safe_div(r.get('total_debt'),   r.get('total_equity')),
        'net_debt_ebitda': safe_div(r.get('total_debt') - r.get('cash', 0), r.get('ebitda')),
        'interest_coverage': safe_div(r.get('ebit'),       r.get('interest_expense')),

        # Efficiency
        'asset_turnover':  safe_div(r.get('revenue'),      r.get('total_assets')),
        'capex_intensity': safe_div(r.get('capex'),        r.get('revenue')),
    }
```

**Step 3 — Present results**

```
FINANCIAL RATIO ANALYSIS — [Company] — [Period]
──────────────────────────────────────────────────────────────
PROFITABILITY              Value    Benchmark     Signal
  Gross Margin:            XX.X%    >50% (SaaS)   [✓/⚠/✗]
  EBITDA Margin:           XX.X%    >20% healthy  [✓/⚠/✗]
  EBIT Margin:             XX.X%
  Net Margin:              XX.X%    >10% healthy  [✓/⚠/✗]
  FCF Margin:              XX.X%    >15% healthy  [✓/⚠/✗]
  ROE:                     XX.X%    >15% good     [✓/⚠/✗]
  ROA:                     XX.X%    >5% good      [✓/⚠/✗]

LIQUIDITY
  Current Ratio:           X.Xx     >1.5 healthy  [✓/⚠/✗]
  Quick Ratio:             X.Xx     >1.0 healthy  [✓/⚠/✗]
  Cash Ratio:              X.Xx     >0.5 safe     [✓/⚠/✗]

LEVERAGE
  Debt/Equity:             X.Xx     <2.0 moderate [✓/⚠/✗]
  Net Debt/EBITDA:         X.Xx     <3.0 safe     [✓/⚠/✗]
  Interest Coverage:       X.Xx     >3.0 healthy  [✓/⚠/✗]

EFFICIENCY
  Asset Turnover:          X.Xx     >0.5 decent   [✓/⚠/✗]
  Capex Intensity:         X.X%     <5% lean      [✓/⚠/✗]

SUMMARY: X of 15 ratios in healthy range. [1-sentence overall assessment]
```

Use industry-appropriate benchmarks. Note when a ratio is N/A due to missing data.
Adjust SaaS benchmarks vs manufacturing vs retail as appropriate for the company.
