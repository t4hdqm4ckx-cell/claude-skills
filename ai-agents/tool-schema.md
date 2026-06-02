---
name: tool-schema
description: Generate a well-formed JSON tool schema for Claude tool use from a natural-language description
---

You are generating a tool definition for Claude's tool use feature. The user will describe what the tool should do. Generate a complete, production-ready tool schema.

**TOOL SCHEMA STRUCTURE**

```python
tool = {
    "name": "snake_case_verb_noun",          # action-oriented: get_weather, search_contracts, send_alert
    "description": "...",                     # 1–3 sentences: what it does, when to use it, what it returns
    "input_schema": {
        "type": "object",
        "properties": {
            "param_name": {
                "type": "string",             # string | number | integer | boolean | array | object
                "description": "...",         # what this param means, valid values, format
                "enum": ["a", "b"],           # if fixed set of values
            },
            "optional_param": {
                "type": "number",
                "description": "...",
                "default": 10,                # document defaults
            }
        },
        "required": ["param_name"],           # only truly required params here
    }
}
```

**DESIGN RULES**

1. **Name is a verb** — `get_`, `search_`, `create_`, `update_`, `delete_`, `send_`, `calculate_`
2. **Description tells Claude WHEN to use it**, not just what it does — "Use this when the user asks about contract renewals within a specific date window"
3. **Mark as few params as required as possible** — Claude is better at choosing defaults than users are at providing them
4. **Use enums for constrained values** — severity levels, status codes, sort directions
5. **Nest objects for grouped params** — don't flatten 10 top-level params; group related ones
6. **Description for each param must include**: what it is + valid range/format + example value
7. **No redundant params** — if a param can be inferred, don't require it

**OUTPUT FORMAT**

```python
# TOOL DEFINITION

tool = {
    "name": "...",
    "description": "...",
    "input_schema": {
        "type": "object",
        "properties": {
            ...
        },
        "required": [...]
    }
}

# USAGE EXAMPLE (Anthropic SDK)
import anthropic

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-sonnet-4-6",
    max_tokens=1024,
    tools=[tool],
    messages=[{"role": "user", "content": "..."}]
)

# Handle tool use
if response.stop_reason == "tool_use":
    tool_use = next(b for b in response.content if b.type == "tool_use")
    result = your_function(**tool_use.input)
    # Continue conversation with tool result...
```

```
DESIGN NOTES:
- [Why this name was chosen]
- [Required vs optional choices]
- [Any params omitted and why]
- [Edge cases the schema handles]
```
