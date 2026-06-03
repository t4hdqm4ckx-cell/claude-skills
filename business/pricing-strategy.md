---
name: pricing-strategy
description: Design or audit SaaS pricing — tier structure, packaging, value metric, anchoring, and expansion revenue levers
---

You are designing or auditing a SaaS pricing strategy.

**Step 1 — Understand the context**

Ask for (or extract):
1. Product type and primary value delivered (e.g., seats, API calls, data volume, outcomes)
2. Current pricing model (flat, per-seat, usage-based, hybrid) — or "greenfield" if new
3. Target customer segments (SMB, Mid-Market, Enterprise, PLG self-serve)
4. Current ARPA and key price points if existing
5. Main competitors and their pricing (if known)
6. Growth goal: acquisition volume, ARPA expansion, or enterprise upsell?

**Step 2 — Framework to apply**

Cover these dimensions:

**Value metric selection** — What should you charge for?
- Seat-based: good for collaboration tools, scales with team size
- Usage-based: good when value ~ consumption (API, storage, compute)
- Outcome-based: good when ROI is measurable (e.g., revenue influenced)
- Hybrid: usage floor + seat cap — best of both

**Tier architecture** — Good/Better/Best:
- 3 tiers is the industry standard; 4 is acceptable for complex products
- Starter: self-serve, frictionless, may be free or low cost
- Growth/Pro: where most SMB/MM revenue lands
- Enterprise: custom, includes security, SSO, SLAs, dedicated CSM

**Anchoring and packaging**:
- Lead with middle tier in UX — anchor the decision
- Put the "pain feature" in Growth/Pro that makes Starter feel incomplete
- Enterprise should feel inevitable for large teams, not a gate

**Expansion levers**:
- Seat expansion (add users)
- Usage overages or tier upgrades
- Add-on modules (SSO, advanced analytics, API access)
- Annual vs monthly uplift (typically 15-20%)

**Step 3 — Output format**

```
PRICING STRATEGY — [Product Name]
─────────────────────────────────────────────────────────────
VALUE METRIC RECOMMENDATION
  Recommended: [metric]
  Rationale:   [why this aligns with customer value and scales with usage]
  Alternative: [second-best option and tradeoff]

TIER ARCHITECTURE

  ┌────────────┬──────────────┬──────────────┬──────────────┐
  │  STARTER   │     PRO      │   BUSINESS   │  ENTERPRISE  │
  │  $[X]/mo   │  $[X]/mo     │  $[X]/mo     │   Custom     │
  ├────────────┼──────────────┼──────────────┼──────────────┤
  │ [Feature]  │ + [Feature]  │ + [Feature]  │ + [Feature]  │
  │ [Feature]  │ + [Feature]  │ + [Feature]  │ + [Feature]  │
  │ [Limit]    │ [Limit]      │ [Limit]      │ Unlimited    │
  └────────────┴──────────────┴──────────────┴──────────────┘

  Anchor tier: [Pro / Business] — [why this tier should dominate revenue]
  Pain feature (drives upgrades): [Feature in Pro that Starter lacks]

ANNUAL PRICING
  Annual discount: [15-20]% (standard) or [X]% (your recommendation)
  Annual attach target: [X]% of customers

EXPANSION REVENUE MODEL
  Primary lever:   [seats / usage / add-ons]
  Est. expansion:  [X]% NRR contribution if [assumption]

COMPETITIVE POSITIONING
  [Competitor A]: [their model] — position [above/below/alongside] because [reason]
  [Competitor B]: [their model] — [positioning rationale]

RISKS & WATCH-OUTS
1. [Risk — e.g., usage-based churn spikes in downturns]
2. [Risk — e.g., Enterprise custom pricing creates sales complexity]

NEXT STEPS
1. [Test/validate recommendation — e.g., A/B test page, customer interviews]
2. [Implementation priority]
```
