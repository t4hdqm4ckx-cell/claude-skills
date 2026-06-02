---
name: fx-exposure
description: Summarize currency exposure across a multi-currency cost base and compute FX sensitivity
---

You are analyzing foreign exchange (FX) exposure.

**Step 1 — Get inputs**
Accept any of:
- A list of costs/revenues by currency (e.g., $500K USD, €200K EUR, £100K GBP)
- A spreadsheet or CSV with `category, currency, monthly_amount`
- Or: a SaaS company's vendor spend file (detect currencies from vendor locations)
- Optional: current FX rates (default to approximate Jun 2026 rates if not provided)
- Optional: functional/reporting currency (default USD)

Approximate reference rates (Jun 2026 — update with user-provided rates if available):
EUR/USD: 1.09, GBP/USD: 1.27, CAD/USD: 0.74, AUD/USD: 0.66, JPY/USD: 0.0068, INR/USD: 0.012

**Step 2 — Compute with Python**
```python
FX_RATES = {'USD': 1.0, 'EUR': 1.09, 'GBP': 1.27, 'CAD': 0.74,
            'AUD': 0.66, 'JPY': 0.0068, 'INR': 0.012}

def to_usd(amount, currency):
    return amount * FX_RATES.get(currency, 1.0)

def fx_sensitivity(amount_usd, shock_pct):
    # How much does USD value change if FX rate moves by shock_pct?
    return amount_usd * shock_pct

exposures = []  # list of {'category': str, 'currency': str, 'monthly': float}

for e in exposures:
    e['monthly_usd'] = to_usd(e['monthly'], e['currency'])
    e['annual_usd']  = e['monthly_usd'] * 12

by_currency = {}
for e in exposures:
    ccy = e['currency']
    by_currency.setdefault(ccy, 0)
    by_currency[ccy] += e['annual_usd']

total_usd = sum(by_currency.values())
for ccy, v in by_currency.items():
    by_currency[ccy] = {'amount_usd': v, 'pct': v / total_usd * 100 if total_usd else 0}

# Sensitivity: impact of 10% FX move
sensitivities = {
    ccy: data['amount_usd'] * 0.10
    for ccy, data in by_currency.items()
    if ccy != 'USD'
}
```

**Step 3 — Present results**

```
FX EXPOSURE ANALYSIS — [Company] — [Date]
Reporting currency: USD  |  Reference rates: [date]
────────────────────────────────────────────────────────
EXPOSURE BY CURRENCY
  Currency   Annual Exposure   % of Total   FX Rate   10% Move Impact
  ────────   ───────────────   ──────────   ───────   ───────────────
  USD        $X,XXX,XXX        XX.X%        1.000     —
  EUR        $XXX,XXX          XX.X%        1.09      $XX,XXX
  GBP        $XXX,XXX           X.X%        1.27      $XX,XXX
  ...
  ──────────────────────────────────────────────────────
  TOTAL      $X,XXX,XXX       100.0%

NON-USD EXPOSURE SUMMARY
  Total non-USD exposure:     $XXX,XXX/yr  (XX.X% of total)
  Largest non-USD currency:   EUR — $XXX,XXX/yr
  
  10% EUR depreciation vs USD:  +$XX,XXX cost impact/yr
  10% GBP depreciation vs USD:  +$XX,XXX cost impact/yr
  Combined 10% FX shock:        +$XX,XXX/yr  (X.X% of total spend)

RISK ASSESSMENT
  [LOW / MEDIUM / HIGH] FX risk — X.X% of costs in non-USD currencies
  
  Hedging consideration: [brief note if exposure >10% of total costs]

NOTE: Analysis based on current spot rates. Actual FX impact depends on 
payment timing, hedging instruments, and rate volatility.
```
