---
name: sql-optimize
description: Rewrite a slow SQL query using indexes, CTEs, window functions, or execution plan guidance
---

You are optimizing a SQL query. The user will provide the query and optionally: the schema, approximate row counts, and the observed execution time or plan.

**STEP 1 — DIAGNOSE**
Before rewriting, identify which of these anti-patterns are present:

| Anti-pattern | Symptom | Fix |
|---|---|---|
| `SELECT *` | Fetches unused columns | Select only needed columns |
| Function on indexed column | `WHERE YEAR(created_at) = 2026` — index bypassed | `WHERE created_at BETWEEN ...` |
| `OR` in WHERE | Often prevents index use | `UNION ALL` of two indexed queries |
| Correlated subquery in SELECT | Runs once per row | `LEFT JOIN` instead |
| `DISTINCT` on large set | Sort + dedup overhead | Check if JOIN produces duplicates — fix the JOIN |
| `NOT IN` with subquery | Slow + NULL-unsafe | `NOT EXISTS` or `LEFT JOIN ... WHERE IS NULL` |
| Missing index on JOIN key | Full table scan | Add index on foreign key |
| `LIKE '%prefix'` | Leading wildcard kills index | Full-text search or redesign |
| Unnecessary subquery | Extra scan | Inline with CTE or JOIN |
| Large `IN (...)` list | Hard to optimize | Temp table or `JOIN` |

**STEP 2 — REWRITE**
Apply fixes. Prefer:
- CTEs (`WITH ...`) for readability over nested subqueries
- Window functions (`ROW_NUMBER`, `SUM OVER`, `LAG`) over self-joins for analytics
- `EXISTS` over `IN` for correlated existence checks
- Batch operations over row-by-row where possible

**OUTPUT FORMAT**

```sql
-- ORIGINAL (annotated with issues)
SELECT ...  -- ❌ SELECT * — fetches unused columns
FROM orders o
WHERE YEAR(o.created_at) = 2026  -- ❌ function on indexed column
  AND customer_id IN (SELECT id FROM customers WHERE status = 'active');  -- ❌ correlated subquery

-- OPTIMIZED
WITH active_customers AS (
    SELECT id FROM customers WHERE status = 'active'  -- materialized once
)
SELECT o.id, o.amount, o.created_at  -- ✓ explicit columns
FROM orders o
JOIN active_customers ac ON o.customer_id = ac.id
WHERE o.created_at >= '2026-01-01' AND o.created_at < '2027-01-01';  -- ✓ range, index-friendly
```

After the rewrite:
```
CHANGES MADE:
1. [Change 1 — why it helps]
2. [Change 2 — why it helps]

INDEXES TO ADD (if schema provided):
  CREATE INDEX idx_orders_created_at ON orders(created_at);
  CREATE INDEX idx_orders_customer_id ON orders(customer_id);

ESTIMATED IMPROVEMENT: [rough estimate if row counts provided, or "significant" / "minor"]

VERIFY WITH: EXPLAIN ANALYZE [optimized query];
```

State the SQL dialect (PostgreSQL / MySQL / SQLite / BigQuery / etc.) and note if the optimization is dialect-specific.
