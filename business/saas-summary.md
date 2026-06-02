---
name: saas-summary
description: Print a concise portfolio summary from the nearest SaaS metrics or renewal agent project
---

You are printing a quick SaaS portfolio summary. Do the following:

1. **Detect which project is active:**
   - If in or near `~/projects/software-renewal-alert-agent/`: use the renewal agent data
   - If in or near `~/projects/saas-metrics-agent/`: use the FlowSync metrics data
   - Otherwise: look for any `data/*.json` or `data/*.xlsx` file nearby

2. **For the renewal agent** (`software_contracts.xlsx`), run:
```python
import sys; sys.path.insert(0, 'agent')
from data_loader import DataLoader
from alerter import Alerter
from utilization_tracker import UtilizationTracker

contracts = DataLoader('data/software_contracts.xlsx').load()
s = Alerter(contracts).summary()
tracker = UtilizationTracker()
waste = tracker.total_annual_waste(contracts)
total = sum(c.get('annual_value',0) for c in contracts)
monthly = sum((c.get('monthly',[{}])[-1].get('actual_spend',0)) for c in contracts if c.get('monthly'))
print(f"Spend: ${total:,.0f}/yr | Monthly actual: ${monthly:,.0f} | Alerts: {s['total']} ({s['critical']} CRITICAL) | Waste: ${waste:,.0f}/yr")
```

3. **For the FlowSync metrics agent** (`synthetic_data.json`), run:
```python
import json
with open('data/synthetic_data.json') as f: d = json.load(f)
last = d['months'][-1]
print(f"MRR: ${last['mrr']:,.0f} | ARR: ${last['arr']:,.0f} | Customers: {last['total_customers']:,} | NRR: {last['nrr']*100:.1f}% | Churn: {last['churn_rate']*100:.2f}%")
```

4. **Format the output** as a single clean line or two-line summary — suitable for pasting into Slack or a status update. No headers, no tables. Just the numbers that matter.

Example output format:
```
[FlowSync · May 2026] MRR $90K · ARR $1.08M · 1,401 customers · NRR 96.3% · Churn 4.0%
[Renewal Agent · Jun 2026] $2.46M portfolio · 77 alerts (6 CRITICAL) · $150K waste · 15 renewals in 90d
```
