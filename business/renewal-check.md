---
name: renewal-check
description: Summarize active CRITICAL and HIGH alerts from the nearest software-renewal-alert-agent data
---

You are performing a quick renewal check. Do the following:

1. Find `data/software_contracts.xlsx` — look first in the current directory, then in `~/projects/software-renewal-alert-agent/`. If not found, say so clearly.

2. Run this Python snippet to extract the current alert summary:

```python
import sys, os
sys.path.insert(0, 'agent')
from data_loader import DataLoader
from renewal_checker import RenewalChecker
from alerter import Alerter
from datetime import date

contracts = DataLoader('data/software_contracts.xlsx').load()
alerter = Alerter(contracts)
s = alerter.summary()
critical = [a for a in s['alerts'] if a.severity == 'CRITICAL']
high = [a for a in s['alerts'] if a.severity == 'HIGH']
print(f"TOTAL: {s['total']}  CRITICAL: {s['critical']}  HIGH: {s['high']}")
for a in critical + high:
    print(f"[{a.severity}] {a.vendor} {a.product} — {a.message}")
```

3. Present the results as a clean summary:
   - Total alert count broken down by severity
   - All CRITICAL alerts listed with vendor, type, and message
   - All HIGH alerts listed
   - One-sentence recommendation for the most urgent action

Keep the response tight — this is a status check, not a full report.
