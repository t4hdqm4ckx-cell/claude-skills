---
name: returns-calc
description: Calculate CAGR, annualized return, Sharpe ratio, and max drawdown from a price or returns series
---

You are computing return metrics from a price or returns series.

**Step 1 — Get the data**
Accept any of:
- A list of prices pasted by the user (e.g., monthly NAV or closing prices)
- A CSV with `date, price` or `date, return` columns
- Direct inputs: start value, end value, number of years, and optionally a risk-free rate

**Step 2 — Compute with Python**
```python
import math

def cagr(start, end, years):
    return (end / start) ** (1 / years) - 1

def annualized_return(returns):
    # returns = list of period returns as decimals (e.g., 0.05 for 5%)
    n = len(returns)
    compound = 1.0
    for r in returns: compound *= (1 + r)
    return compound ** (12 / n) - 1  # assumes monthly; adjust for daily/annual

def sharpe(returns, risk_free_rate=0.045):
    # risk_free_rate default = 4.5% (approximate 2026 T-bill rate)
    import statistics
    mean_r = sum(returns) / len(returns)
    std_r  = statistics.stdev(returns) if len(returns) > 1 else 0
    excess = mean_r - (risk_free_rate / 12)  # monthly excess
    return (excess / std_r) * (12 ** 0.5) if std_r else 0  # annualized

def max_drawdown(prices):
    peak, max_dd = prices[0], 0
    for p in prices:
        if p > peak: peak = p
        dd = (p - peak) / peak
        if dd < max_dd: max_dd = dd
    return max_dd

def volatility(returns):
    import statistics
    return statistics.stdev(returns) * (12 ** 0.5)  # annualized monthly vol
```

**Step 3 — Present results**

```
RETURN METRICS
──────────────────────────────────
Period:            [start date] → [end date]  (N months / Y years)
Total Return:      +X.X%
CAGR:              +X.X% per year
Annualized Return: +X.X%
Annualized Vol:    X.X%
Sharpe Ratio:      X.XX  (rf = 4.5%)
Max Drawdown:      -X.X%  (peak: $X → trough: $X)

INTERPRETATION
  Sharpe > 1.0 = good  |  > 2.0 = excellent  |  < 0.5 = poor
  Max DD context: [brief note on severity]
```

Use a default risk-free rate of 4.5% (approximate 2026 US T-bill). If the user provides a different rate, use that.
Always state the number of periods and whether returns are monthly/daily/annual.
