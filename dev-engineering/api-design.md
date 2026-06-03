---
name: api-design
description: Design REST or GraphQL API endpoints — routes, request/response schemas, auth, versioning, and error handling conventions
---

You are designing an API. The user will describe what the API needs to do.

**Step 1 — Clarify scope**

Ask (or infer from context):
1. REST or GraphQL? (or gRPC — adjust output accordingly)
2. What resource(s) does this API expose?
3. Who are the consumers? (internal services, mobile app, third-party partners)
4. Authentication method: API key, JWT/OAuth2, session cookie, mTLS?
5. Any existing conventions in the codebase to follow?

**Step 2 — Design principles to apply**

- **Nouns, not verbs** in REST URLs: `/orders`, not `/getOrders`
- **HTTP verbs map to CRUD**: GET (read), POST (create), PUT/PATCH (update), DELETE (remove)
- **PATCH vs PUT**: PATCH for partial update, PUT for full replacement
- **Plural resource names**: `/users/{id}`, `/orders/{id}/items`
- **Consistent error format**: always return structured errors (code, message, details)
- **Versioning**: path-based (`/v1/`) preferred for public APIs; header-based for internal
- **Pagination**: cursor-based for large/real-time datasets, offset for simple cases
- **Idempotency**: PUT and DELETE should be idempotent; POST should accept idempotency keys for critical ops

**Step 3 — Output format**

```
API DESIGN — [Resource/Feature Name]
─────────────────────────────────────────────────────────────
BASE URL:  /v1/[resource]
AUTH:      [Bearer JWT / API Key header: X-Api-Key / OAuth2 scope: X]

ENDPOINTS

  [METHOD] [PATH]
  Description: [what it does]
  Auth required: [yes/no] | Scope: [scope if OAuth]

  Request:
    Path params:  { id: string }
    Query params: { page?: number, limit?: number (default 20, max 100) }
    Body (JSON):
    {
      "field": type,   // required — description
      "field"?: type   // optional — description
    }

  Response 200:
    {
      "id": "string",
      "field": type,
      "createdAt": "ISO8601"
    }

  Errors:
    400  Bad Request    — validation failure; body: { "code": "INVALID_INPUT", "details": [...] }
    401  Unauthorized   — missing or invalid token
    403  Forbidden      — authenticated but lacks permission
    404  Not Found      — resource does not exist
    409  Conflict       — duplicate / state violation
    429  Too Many Reqs  — rate limited; include Retry-After header
    500  Server Error   — never expose internal details

[Repeat for each endpoint]

─────────────────────────────────────────────────────────────
PAGINATION (if applicable)
  Strategy: [cursor / offset]
  Response envelope:
  {
    "data": [...],
    "pagination": {
      "nextCursor": "string | null",   // cursor-based
      "total": number,                 // offset-based
      "page": number,
      "limit": number
    }
  }

RATE LIMITING
  Limit: [X] requests per [minute/hour] per [API key / user / IP]
  Headers: X-RateLimit-Limit, X-RateLimit-Remaining, X-RateLimit-Reset

VERSIONING STRATEGY
  [Path-based /v1/ — bump to /v2/ on breaking changes]
  [Deprecation notice via Sunset header]

OPEN QUESTIONS
  □ [Decision needed from user — e.g., "should DELETE be soft or hard?"]
  □ [e.g., "confirm auth scope names with identity team"]
```

For GraphQL, replace the REST section with schema-first type definitions, query/mutation names, and resolver notes.
