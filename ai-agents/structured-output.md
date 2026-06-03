---
name: structured-output
description: Write prompt instructions that enforce structured JSON output — schema definition, validation rules, and fallback handling
---

You are writing prompt instructions to enforce structured JSON output from a language model.

**Step 1 — Understand the target output**

Ask for (or extract):
1. What data needs to be in the output? (fields, types, nesting)
2. Which fields are required vs optional?
3. What model/API is being used? (Claude, GPT-4, Gemini — affects technique)
4. Is this a one-shot prompt or part of a pipeline where the JSON is parsed programmatically?
5. Are there enum constraints, min/max values, or format rules (e.g., ISO date, email)?

**Step 2 — Design the JSON schema**

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "required": ["field1", "field2"],
  "properties": {
    "field1": {
      "type": "string",
      "description": "..."
    },
    "field2": {
      "type": "number",
      "minimum": 0
    },
    "field3": {
      "type": "string",
      "enum": ["option_a", "option_b", "option_c"]
    },
    "field4": {
      "type": "array",
      "items": { "type": "string" },
      "minItems": 1
    },
    "nested": {
      "type": "object",
      "required": ["sub_field"],
      "properties": {
        "sub_field": { "type": "boolean" }
      }
    }
  },
  "additionalProperties": false
}
```

**Step 3 — Generate the prompt instructions**

Produce the following artifacts:

**A. System prompt instruction block**

```
You must respond with valid JSON only. Do not include any text before or after the JSON.
Do not include markdown code fences (no ```json).

Your response must conform to this schema:
{
  "field1": string,          // required — [description]
  "field2": number,          // required — [description]; must be >= 0
  "field3": "option_a" | "option_b" | "option_c",   // required
  "field4": string[],        // required — at least one item
  "nested": {
    "sub_field": boolean     // required
  }
}

If you cannot determine a value for a required field, use null and include an
"errors" array explaining which fields could not be populated and why.
```

**B. Validation code snippet (Python)**

```python
import json, jsonschema

SCHEMA = { ... }  # paste schema here

def parse_response(raw: str) -> dict:
    try:
        data = json.loads(raw.strip())
    except json.JSONDecodeError as e:
        raise ValueError(f"Model returned invalid JSON: {e}\nRaw: {raw[:200]}")
    jsonschema.validate(instance=data, schema=SCHEMA)
    return data
```

**C. Fallback handling instructions**

Add to prompt when strict compliance is critical:
```
If you are unable to produce valid JSON matching the schema, respond with exactly:
{"error": "unable_to_comply", "reason": "[brief explanation]"}
```

**Step 4 — Output summary**

```
STRUCTURED OUTPUT DESIGN
─────────────────────────────────────────────────────────────
Schema:           [X required fields, Y optional fields]
Nesting depth:    [flat / 1 level / 2+ levels]
Enum constraints: [list any]
Null policy:      [null allowed / not allowed — state which fields]

TECHNIQUE RECOMMENDATIONS
  Claude tool use:  [yes/no — use if API supports it; most reliable]
  Prefill trick:    Start assistant turn with `{` to force JSON mode
  Validation:       [library recommended — jsonschema / Pydantic / zod]

KNOWN FAILURE MODES
  □ Model wraps JSON in markdown fences — strip with regex: r'```(?:json)?\n?(.*?)\n?```'
  □ Model adds explanation before JSON — strip with: raw[raw.index('{'):]
  □ Enum value hallucination — add: "Only use exact strings listed; do not invent values"
```
