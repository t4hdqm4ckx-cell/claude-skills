---
name: rebalance-check
description: Compare current vs target allocation and output exact shares to buy/sell to rebalance
---

You are computing a rebalance plan. 

**Step 1 — Get inputs**
Ask the user for (or extract from their message):
- Current holdings: ticker, shares, current price
- Target allocation: ticker → target % (must sum to 100%)
- Total portfolio value (or compute from holdings)
- Optional: minimum trade threshold (default $500 — ignore smaller rebalances)
- Optional: tax-sensitive mode (prefer sells only if losses, prefer buys first)

**Step 2 — Compute with Python**
```python
holdings = [
    # {'ticker': 'AAPL', 'shares': 10, 'price': 185.0},
    # ...
]
targets = {
    # 'AAPL': 0.25,  # 25%
    # ...
}
MIN_TRADE = 500  # ignore trades smaller than this

total_value = sum(h['shares'] * h['price'] for h in holdings)

rebalance = []
for h in holdings:
    ticker = h['ticker']
    current_value  = h['shares'] * h['price']
    current_pct    = current_value / total_value
    target_pct     = targets.get(ticker, 0)
    target_value   = total_value * target_pct
    delta_value    = target_value - current_value
    delta_shares   = delta_value / h['price']
    drift          = current_pct - target_pct

    if abs(delta_value) >= MIN_TRADE:
        rebalance.append({
            'ticker': ticker,
            'current_pct': current_pct * 100,
            'target_pct': target_pct * 100,
            'drift': drift * 100,
            'action': 'BUY' if delta_shares > 0 else 'SELL',
            'shares': abs(round(delta_shares, 2)),
            'value': abs(round(delta_value, 2)),
        })

rebalance.sort(key=lambda x: abs(x['drift']), reverse=True)
```

**Step 3 — Present results**

```
REBALANCE PLAN — Portfolio Value: $X,XXX,XXX
Min trade threshold: $500
──────────────────────────────────────────────────────────
TICKER   Current%   Target%   Drift    Action   Shares   Value
──────   ────────   ───────   ─────    ──────   ──────   ─────
AAPL     28.3%      25.0%     +3.3%    SELL     1.8      $333
BND      12.1%      20.0%     -7.9%    BUY      22.0     $1,980
...

SUMMARY
  Total buys:   $X,XXX across N positions
  Total sells:  $X,XXX across N positions
  Net cash needed: $+/- XXX
  
  X positions within threshold (no action needed)
  Estimated trades after rebalance: X
```

Note any positions not in the target allocation (candidates for full liquidation).
Flag if targets don't sum to 100%.
