---
name: options-pricing
description: Price a European call or put option using Black-Scholes — output premium, Greeks, and break-even analysis
---

You are pricing an options contract using the Black-Scholes model.

**Step 1 — Gather inputs**

Ask for (or extract from the user's message):
- Underlying asset and current price (S)
- Strike price (K)
- Time to expiration in days (convert to years: T = days/365)
- Risk-free rate (%) — use current T-bill rate if not provided; state assumption
- Implied volatility (%) — if unknown, ask for historical vol or flag it
- Option type: Call or Put
- American or European? (Black-Scholes is for European; flag if American)

**Step 2 — Calculate**

Black-Scholes formula:
- d1 = [ln(S/K) + (r + σ²/2) × T] / (σ × √T)
- d2 = d1 - σ × √T
- Call = S × N(d1) - K × e^(-rT) × N(d2)
- Put = K × e^(-rT) × N(-d2) - S × N(-d1)

Greeks:
- Delta: N(d1) for call, N(d1) - 1 for put
- Gamma: N'(d1) / (S × σ × √T)
- Theta: -(S × N'(d1) × σ) / (2√T) - r × K × e^(-rT) × N(d2) [call]
- Vega: S × N'(d1) × √T
- Rho: K × T × e^(-rT) × N(d2) [call]

**Step 3 — Output format**

```
OPTIONS PRICING — [Ticker] [Strike] [Expiry] [Call/Put]
─────────────────────────────────────────────────────────────
INPUTS
  Spot price:        $[S]
  Strike:            $[K]    Moneyness: [ITM/ATM/OTM]
  Days to expiry:    [N] days ([T] years)
  Risk-free rate:    [r]%
  Implied vol:       [σ]%

OPTION PREMIUM
  Theoretical price: $[X]
  Intrinsic value:   $[X]
  Time value:        $[X]

GREEKS
  Delta:   [X]   (price change per $1 move in underlying)
  Gamma:   [X]   (delta change per $1 move)
  Theta:   -$[X]/day  (daily time decay)
  Vega:    $[X] per 1% vol move
  Rho:     $[X] per 1% rate move

BREAK-EVEN AT EXPIRY
  Break-even price:   $[K ± premium]
  Required move:      [X]% [up/down] from current price

ASSUMPTIONS
  Model: Black-Scholes (European exercise) | Vol: [stated/estimated]
─────────────────────────────────────────────────────────────
INTERPRETATION
[2-3 sentences: is the option cheap/expensive vs historical vol, key risk (theta decay vs delta gain), practical takeaway]
```

**Caveats to always mention**
- Black-Scholes assumes constant volatility (vol smile/skew not captured)
- American options may have early exercise value not reflected here
- Output is for educational analysis, not trading advice
