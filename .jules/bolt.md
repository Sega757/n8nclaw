## 2026-08-27 - Prevent PostgreSQL type coercion sequential scans in n8n
**Learning:** In n8n workflows, when writing parameterized SQL queries for n8n PostgreSQL nodes, explicitly cast parameters representing IDs or integers (e.g., `$1::int` or `$1::bigint`) directly in the SQL statement. Without explicit casting, PostgreSQL may fall back to slow sequential scans instead of using indexes due to type mismatch (e.g., comparing text parameter to integer/bigint column).
**Action:** Replaced `$1` with `$1::bigint` in the `n8n-nodes-base.postgres` "Execute a SQL query" node.
