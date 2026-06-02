---
name: regex-build
description: Build and explain a regex pattern from a plain-English description with test cases
---

You are building a regular expression. The user will describe what they want to match in plain English, optionally with examples.

**PROCESS**

1. **Clarify the target** — if the description is ambiguous, state your interpretation before building
2. **Build incrementally** — construct the pattern piece by piece, not all at once
3. **Test against examples** — verify against provided examples and common edge cases
4. **Explain every part** — annotate the pattern with inline comments

**OUTPUT FORMAT**

```
PATTERN (Python re / JavaScript / PCRE — state which):
  /your-pattern-here/flags

BREAKDOWN:
  ^           — start of string (if anchored)
  [A-Z]{2}    — exactly 2 uppercase letters
  \d{4}       — exactly 4 digits
  [-./]       — literal hyphen, dot, or slash
  $           — end of string (if anchored)

MATCHES (should match):
  ✓ "AB1234"
  ✓ "XY9999"

DOES NOT MATCH (should not match):
  ✗ "ab1234"  — lowercase not allowed
  ✗ "A1234"   — only 1 letter, need 2
  ✗ "AB12345" — 5 digits, not 4

PYTHON USAGE:
  import re
  pattern = re.compile(r'YOUR_PATTERN', re.IGNORECASE)
  match = pattern.match(text)
  # or for all occurrences:
  results = pattern.findall(text)

JAVASCRIPT USAGE:
  const pattern = /YOUR_PATTERN/g;
  const matches = text.match(pattern);

GOTCHAS:
  - [Any edge cases to watch for]
  - [Greedy vs lazy matching note if relevant]
  - [Unicode/encoding note if relevant]
```

Always provide both Python and JavaScript usage. Always test at least 3 valid and 3 invalid examples.
If the pattern is complex, suggest breaking it into named groups:
```python
pattern = re.compile(
    r'(?P<area_code>\d{3})'   # 3-digit area code
    r'[-.]'                    # separator
    r'(?P<number>\d{7})'      # 7-digit number
)
```
