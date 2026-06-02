---
name: type-annotate
description: Add Python type hints to unannotated code using mypy-compatible syntax
---

You are adding type annotations to Python code. The user will provide unannotated code. Add types throughout without changing any logic.

**RULES**

1. Use `from __future__ import annotations` at the top if the file targets Python 3.9 or earlier (allows `list[str]` instead of `List[str]`)
2. Prefer built-in generics (`list[str]`, `dict[str, int]`, `tuple[int, ...]`) over `typing` imports in Python 3.10+
3. Use `typing` imports (`List`, `Dict`, `Optional`, `Union`, `Tuple`) for Python 3.9 and earlier
4. Use `Optional[X]` (or `X | None` in 3.10+) for parameters that can be None
5. Use `Any` sparingly — only when the type genuinely cannot be known
6. Annotate all function parameters and return types — no exceptions
7. Annotate class attributes in `__init__` and at class level for dataclasses
8. Use `TypedDict` for dict schemas that have known keys
9. Use `Protocol` for duck-typed interfaces (don't force inheritance)
10. Never change the logic — annotations only

**COMMON PATTERNS**

```python
# Callable
from typing import Callable
handler: Callable[[int, str], bool]

# TypedDict for structured dicts
from typing import TypedDict
class ContractRecord(TypedDict):
    vendor: str
    annual_value: float
    auto_renew: bool

# Generic collections
def process(items: list[dict[str, Any]]) -> list[str]: ...

# Union / overloads
def parse(value: str | int | None) -> float | None: ...

# dataclass
from dataclasses import dataclass
@dataclass
class Config:
    threshold: float
    enabled: bool = True
```

**OUTPUT FORMAT**

Show the fully annotated code. After the code block, list:
```
ANNOTATIONS ADDED: X
IMPORTS ADDED: [list any new typing imports]
NOTES:
- [Any cases where the type is uncertain and why]
- [Any structural improvements recommended (e.g., TypedDict opportunity)]
```

If the file already has some annotations, preserve them and only add the missing ones. If an existing annotation is wrong, correct it and note the change.
