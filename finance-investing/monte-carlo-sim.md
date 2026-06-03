---
name: monte-carlo-sim
description: Run a Monte Carlo simulation on a portfolio or investment — output return distribution, percentiles, and probability of loss
---

You are running a Monte Carlo simulation for portfolio or investment return analysis.

**Step 1 — Gather inputs**

Ask the user for (or extract from their message):
- Asset(s) or portfolio composition (e.g., 60% equities, 40% bonds)
- Expected annual return per asset (%)
- Annual volatility / standard deviation per asset (%)
- Correlation between assets (if multi-asset; default to 0 if unknown)
- Investment horizon (years)
- Starting value ($)
- Number of simulations (default: 10,000)
- Any annual contribution or withdrawal (optional)

**Step 2 — Simulation logic (describe, don't code unless asked)**

Use a lognormal return model:
- Each year: r ~ Normal(μ - σ²/2, σ) where μ = expected return, σ = volatility
- Compound across years for each simulation path
- Aggregate final values across all paths

**Step 3 — Output format**

```
MONTE CARLO SIMULATION — [Portfolio/Asset Name]
Horizon: [N] years | Simulations: [N] | Starting value: $[X]
─────────────────────────────────────────────────────────────
RETURN DISTRIBUTION (Final Portfolio Value)

  Percentile    Final Value    Total Return
  ──────────    ───────────    ────────────
  5th (Worst)   $[X]           [X]%
  25th          $[X]           [X]%
  50th (Median) $[X]           [X]%
  75th          $[X]           [X]%
  95th (Best)   $[X]           [X]%

RISK METRICS
  Probability of loss (< $[start]):   [X]%
  Probability of 2x:                  [X]%
  Expected value (mean):              $[X]
  Value at Risk (95% confidence):     -$[X] ([X]%)

ASSUMPTIONS
  Expected return: [X]% | Volatility: [X]% | Model: lognormal
─────────────────────────────────────────────────────────────
INTERPRETATION
[2-3 sentences on what the distribution means practically: best/worst cases, key risk, how to act on results]
```

**Notes**
- If the user provides historical data, derive μ and σ from it
- For multi-asset portfolios, apply correlation matrix via Cholesky decomposition (mention this)
- Flag if volatility inputs seem inconsistent with asset class norms
- Always remind user this is a model — past correlations and volatility may not persist
