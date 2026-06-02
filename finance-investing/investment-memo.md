---
name: investment-memo
description: Generate a structured 1-page investment memo — thesis, moat, risks, valuation, catalysts
---

You are generating a structured investment memo. This is a concise, opinionated document that captures the core investment thesis and key considerations.

**Step 1 — Get inputs**
The user will provide one of:
- Company name + brief description
- Ticker + financial data
- A longer brief they want structured into memo format

Ask for any missing critical inputs: current price, market cap, revenue, growth rate, and the user's investment thesis in 1–2 sentences.

**Step 2 — Generate the memo**

Use this exact template:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
INVESTMENT MEMO — [COMPANY NAME] ([TICKER])
[Date]  |  Sector: [X]  |  Market Cap: $XB  |  Price: $XX.XX
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

THESIS (1-2 sentences)
[The single clearest statement of why this is an interesting investment.
What is the market missing or mispricing?]

BUSINESS OVERVIEW
[2–3 sentences: what the company does, who the customer is, how it makes money,
and its stage of development (growth, mature, turnaround, etc.)]

COMPETITIVE MOAT
□ Network effects        □ Switching costs       □ Cost advantages
□ Intangible assets      □ Efficient scale        □ None identified

[2–3 sentences describing the primary moat and how durable it is.]

FINANCIAL SNAPSHOT
  Revenue (TTM):    $XB    YoY growth: +XX%
  Gross Margin:     XX%    EBITDA Margin: XX%
  FCF Margin:       XX%    Net Debt/EBITDA: X.Xx
  EV/Revenue:       X.Xx   EV/EBITDA: XX.Xx   P/E: XX.Xx

VALUATION
[Current valuation vs. peer median. DCF implied value if computed.
Is the stock cheap, fairly valued, or expensive? On what metric?]
  Base case price target: $XX.XX  ([X%] upside/downside)
  Bull case: $XX.XX  |  Bear case: $XX.XX

CATALYSTS (what could make the stock move)
1. [Near-term, 0–6 months]
2. [Medium-term, 6–18 months]
3. [Long-term, 18+ months]

KEY RISKS
1. [Biggest risk — one sentence]
2. [Second risk]
3. [Third risk]

WHAT WOULD CHANGE THE THESIS
[1–2 sentences: the specific data point or event that would make this investment
thesis wrong. This is the most important section for intellectual honesty.]

RECOMMENDATION
[BUY / HOLD / SELL / WATCH]  at [$XX.XX or below / current price / ...]
Position sizing suggestion: [Core / Starter / Avoid] based on conviction

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
DISCLAIMER: This memo is for informational purposes only. Not financial advice.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Rules:**
- The thesis must be one or two sentences — not a paragraph
- "What would change the thesis" is mandatory — skip nothing
- Moat checkboxes: only check boxes that are genuinely present, not aspirational
- Price target must have a basis (DCF, comps, or historical multiple) — state which
- Keep total length to one page (print equivalent)
- Always append the disclaimer
