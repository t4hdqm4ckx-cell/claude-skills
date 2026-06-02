---
name: var-calc
description: Value-at-Risk estimate — parametric or historical VaR from a returns series
---

You are computing Value-at-Risk (VaR) and related risk metrics.

**Step 1 — Get inputs**
Accept any of:
- A returns series (list of daily or monthly returns as decimals)
- A CSV with `date, return` columns
- Portfolio value + asset weights + individual return series
- Optional: confidence level (default 95%), time horizon (default 1 day)
- Optional: method preference (parametric / historical / both)

**Step 2 — Compute with Python**
```python
import statistics, math

def parametric_var(returns, confidence=0.95, horizon_days=1, portfolio_value=1):
    mean = statistics.mean(returns)
    std  = statistics.stdev(returns)
    # Z-scores: 90%=1.282, 95%=1.645, 99%=2.326
    z = {0.90: 1.282, 0.95: 1.645, 0.99: 2.326}.get(confidence, 1.645)
    daily_var_pct = -(mean - z * std) * math.sqrt(horizon_days)
    return daily_var_pct * portfolio_value, daily_var_pct

def historical_var(returns, confidence=0.95, portfolio_value=1):
    sorted_r = sorted(returns)
    idx = int((1 - confidence) * len(sorted_r))
    var_pct = -sorted_r[idx]
    return var_pct * portfolio_value, var_pct

def expected_shortfall(returns, confidence=0.95, portfolio_value=1):
    # CVaR — average loss beyond VaR threshold
    sorted_r = sorted(returns)
    cutoff = int((1 - confidence) * len(sorted_r))
    tail = sorted_r[:cutoff]
    es_pct = -statistics.mean(tail) if tail else 0
    return es_pct * portfolio_value, es_pct

def annualized_vol(daily_returns):
    return statistics.stdev(daily_returns) * math.sqrt(252)

def max_drawdown(prices_or_cumulative_returns):
    peak, max_dd = prices_or_cumulative_returns[0], 0
    for p in prices_or_cumulative_returns:
        if p > peak: peak = p
        dd = (p - peak) / peak
        if dd < max_dd: max_dd = dd
    return max_dd
```

**Step 3 — Present results**

```
VALUE AT RISK ANALYSIS — [Asset/Portfolio] — [Date]
────────────────────────────────────────────────────────
DATA SUMMARY
  Observations:       N returns  (daily / monthly)
  Period:             [start] → [end]
  Mean daily return:  +X.XXX%
  Daily volatility:   X.XXX%
  Annualized vol:     XX.X%

VALUE AT RISK (Portfolio value: $X,XXX,XXX)
                        95% Confidence    99% Confidence
  Parametric VaR (1d):  $X,XXX  (X.XX%)  $X,XXX  (X.XX%)
  Historical VaR (1d):  $X,XXX  (X.XX%)  $X,XXX  (X.XX%)
  
  10-day VaR (95%):     $XX,XXX (X.XX%)
  Monthly VaR (95%):    $XX,XXX (X.XX%)

TAIL RISK
  Expected Shortfall (CVaR, 95%):  $X,XXX  (X.XX%)
  Max Drawdown (historical):       -XX.X%

INTERPRETATION
  "On any given day, there is a 95% chance that the portfolio
   will not lose more than $X,XXX ($X.XX%)."
  
  CVaR tells you: when you do breach VaR, the average loss is $X,XXX.

NOTE: VaR assumes [normal distribution / historical distribution].
Past return distributions may not predict future tail events.
```

Always compute both parametric and historical VaR when enough data is available (n>30).
Note any fat-tail or skewness concerns if the historical distribution is non-normal.
