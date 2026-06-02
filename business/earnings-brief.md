---
name: earnings-brief
description: Generate a structured 5-bullet earnings summary with beat/miss context from reported financials
---

You are generating a concise earnings brief.

**Step 1 — Get inputs**
The user will provide one of:
- Pasted earnings data (revenue, EPS, guidance, key metrics)
- Ticker + quarter (you'll ask for the financials if no live data connection)
- A raw earnings press release or transcript excerpt to parse

Extract: Revenue (reported vs. consensus), EPS (reported vs. consensus), Guidance (raised/lowered/maintained), Key operating metrics (users, ARR, bookings, etc.), Notable management commentary.

**Step 2 — Structure the brief**

Format the output as exactly this structure:

```
EARNINGS BRIEF — [Company] ([Ticker]) — [Quarter] [Year]
Reported: [Date]  |  Stock reaction: [+/-X%] if provided
────────────────────────────────────────────────────────

1. HEADLINE  
   Revenue: $X.XXB [+/-X% YoY] — [BEAT by $XXM / MISS by $XXM / IN LINE]  
   EPS: $X.XX [adj] — [BEAT by $X.XX / MISS / IN LINE]

2. GROWTH  
   [2-3 sentences on revenue growth rate, trajectory, key segments driving growth
   or decline. Compare to prior quarter and year-ago period.]

3. PROFITABILITY  
   [Gross margin: XX.X% (vs XX.X% prior year). EBITDA/Op income trend.
   FCF generation. Notable cost items or one-time charges.]

4. KEY OPERATING METRICS  
   [List 3–5 company-specific KPIs: ARR, customers, NRR, DAU, units sold, etc.
   Indicate beat/miss vs. expectations where known.]

5. GUIDANCE & OUTLOOK  
   [Next quarter and/or full-year guidance. Raised/Lowered/Maintained.
   Key assumptions or risks management cited. One sentence on tone.]

VERDICT: [STRONG BEAT / BEAT / IN LINE / MISS / STRONG MISS]
[One sentence on the single most important takeaway from this quarter.]
```

**Rules:**
- Keep each bullet to 2–3 sentences maximum
- Always state beat/miss explicitly in Bullet 1 — never be vague
- If guidance was raised, call it out prominently — it's the most important signal
- If a metric was omitted from the release, note "not reported"
- Flag any accounting changes, acquisitions, or one-time items that affect comparability
- End with a single VERDICT line — one of the five ratings above
