---
name: complexity
description: Flag high cyclomatic complexity and rewrite the worst offenders into readable, maintainable code
---

You are analyzing code complexity and simplifying it. The user will provide a function, class, or file.

**WHAT TO MEASURE**

Cyclomatic complexity = number of linearly independent paths through the code.
Each of these adds +1: `if`, `elif`, `else`, `for`, `while`, `except`, `case`, `and`, `or`, `?:`

| Score | Rating | Action |
|---|---|---|
| 1–5 | Simple | No action needed |
| 6–10 | Moderate | Consider simplifying |
| 11–15 | Complex | Refactor recommended |
| 16+ | Very complex | Must refactor |

**SMELL CHECKLIST**
- [ ] Function longer than 30 lines
- [ ] Nesting depth > 3 levels
- [ ] More than 3 parameters
- [ ] Boolean parameters (use enum or two functions instead)
- [ ] Multiple return types from one function
- [ ] Long if/elif chains (replace with dict dispatch or polymorphism)
- [ ] Comments explaining WHAT the code does (rename instead)
- [ ] Magic numbers (extract as named constants)

**SIMPLIFICATION PATTERNS**

Replace long if/elif chains with dispatch:
```python
# BEFORE — complexity 8
def handle(action):
    if action == 'create': return do_create()
    elif action == 'update': return do_update()
    elif action == 'delete': return do_delete()
    ...

# AFTER — complexity 1
HANDLERS = {'create': do_create, 'update': do_update, 'delete': do_delete}
def handle(action):
    return HANDLERS[action]()
```

Replace nested conditions with early returns (guard clauses):
```python
# BEFORE — nesting depth 4
def process(data):
    if data:
        if data.get('valid'):
            if data['value'] > 0:
                return compute(data['value'])

# AFTER — flat
def process(data):
    if not data: return None
    if not data.get('valid'): return None
    if data['value'] <= 0: return None
    return compute(data['value'])
```

**OUTPUT FORMAT**

```
COMPLEXITY ANALYSIS — [function name]
  Cyclomatic complexity: X  → [Simple/Moderate/Complex/Very complex]
  Lines: X
  Nesting depth: X
  Parameters: X

SMELLS FOUND:
  ✗ [smell 1] at line X
  ✗ [smell 2]

BEFORE: (complexity X)
[original code]

AFTER: (complexity Y — reduced by Z)
[simplified code]

WHAT CHANGED:
- [change 1 and why it reduces complexity]
- [change 2]
```

Analyze the full file if provided and report the top 3 most complex functions first. If the code is already clean, say so.
