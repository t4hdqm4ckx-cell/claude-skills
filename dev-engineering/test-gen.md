---
name: test-gen
description: Generate a complete test suite for a function or module — happy path, edge cases, failure cases
---

You are generating a test suite. The user will provide a function, class, or module. Generate thorough tests without asking unnecessary questions.

**WHAT TO COVER — in this order:**

1. **Happy path** — normal inputs producing expected outputs
2. **Boundary values** — min/max valid inputs, empty collections, zero, single-element
3. **Type edge cases** — None inputs, wrong types (if not type-annotated), empty strings
4. **Error cases** — inputs that should raise exceptions; verify the right exception is raised
5. **State side effects** — if the function modifies state, verify before AND after
6. **Idempotency** — if calling twice should give the same result, test it
7. **Integration** — if the function calls I/O (file, network, DB), test with real data when possible; mock only when I/O is unavoidable

**RULES**
- Use the same test framework already in the project (detect from imports or ask). Default: plain `assert` with a `if __name__ == '__main__'` runner — no pytest required
- Each test function tests exactly ONE thing
- Test names must be descriptive: `test_variance_pct_returns_zero_when_no_monthly_data` not `test_1`
- No mocking of internal logic — only mock external I/O (file reads, HTTP calls, DB)
- If a test requires setup data, create it inline — no shared fixtures unless the project already uses them
- Add a comment only when the test case is non-obvious (e.g., a known edge case from a past bug)

**OUTPUT FORMAT**
```python
"""Tests for [module/function name]."""
import sys, os
# [path setup if needed]

# [any test fixtures or shared data — keep minimal]

def test_[descriptive_name]():
    # [setup if needed]
    result = function_under_test(inputs)
    assert result == expected, f"Expected {expected}, got {result}"

# [more tests...]

if __name__ == '__main__':
    tests = [test_1, test_2, ...]  # list all test functions
    passed = 0
    for t in tests:
        try:
            t(); print(f"  ✓ {t.__name__}"); passed += 1
        except AssertionError as e:
            print(f"  ✗ {t.__name__}: {e}")
    print(f"\n{passed}/{len(tests)} tests passed")
```

After generating, state: "X tests covering Y scenarios. Missing coverage: [list any important cases not covered due to insufficient information about the code]."
