## 2024-05-18 - Fix SQL Injection in n8n Postgres Node
**Vulnerability:** SQL Injection in `n8nClaw.json` Postgres node.
**Learning:** n8n workflow nodes often allow direct string interpolation (e.g., `{{ $json.field }}`) in SQL queries, leading to SQL injection if not parameterized properly.
**Prevention:** Always use parameterized queries (e.g., `$1`, `$2` and `queryParameters`) in n8n Postgres nodes instead of directly injecting dynamic values.
