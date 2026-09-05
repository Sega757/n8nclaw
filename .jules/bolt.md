## 2026-08-27 - Prevent PostgreSQL type coercion sequential scans in n8n
**Learning:** In n8n workflows, when writing parameterized SQL queries for n8n PostgreSQL nodes, explicitly cast parameters representing IDs or integers (e.g., `$1::int` or `$1::bigint`) directly in the SQL statement. Without explicit casting, PostgreSQL may fall back to slow sequential scans instead of using indexes due to type mismatch (e.g., comparing text parameter to integer/bigint column).
**Action:** Replaced `$1` with `$1::bigint` in the `n8n-nodes-base.postgres` "Execute a SQL query" node.

## 2026-08-27 - PostgreSQL large payloads limit for Data Loaders
**Learning:** Omitting `LIMIT` in a scheduled batch processing workflow's PostgreSQL query causes massive payloads, but appending it naturally leverages the existing cursor (e.g., `last_vector_id`) to create a robust pagination loop. This implements pagination for the data loader, fetching records in batches instead of pulling the entire dataset into memory. This reduces memory usage and prevents LLM token limit exhaustion when processing large numbers of historic chat messages.
**Action:** Appended `LIMIT 50` to the SQL query in the Postgres node `3c618b36-eaa2-42ff-b28d-befc276f0038`.
## 2026-08-27 - Pre-filtering at source for polling triggers in n8n
**Learning:** For polling triggers like the Gmail Trigger in n8n, relying exclusively on downstream filters (e.g., a Filter node checking sender email) is inefficient. n8n will fetch all incoming items (costing API calls, bandwidth, and memory) only to discard them downstream. By utilizing native filtering parameters (like the `q` search parameter in Gmail) on the trigger node itself, you prevent unauthorized or irrelevant data from ever entering the workflow, saving resources.
**Action:** Added `from:={{ $env.AUTHORIZED_EMAIL }}` to the `q` filter parameter on the Gmail Trigger node to block unauthorized emails before they trigger workflow executions.

## 2026-08-27 - Optimize Gmail AI tools for LLM token efficiency
**Learning:** In n8n workflows, when an AI Agent uses Gmail tools (like `Get a message in Gmail` or `Get many messages in Gmail`) with `"simple": false`, n8n returns raw, unparsed email payloads (including MIME boundaries, large HTML blocks, and SMTP headers) directly to the LLM. This massive payload severely wastes tokens, drastically increases API latency, and can cause context window exhaustion. By setting `"simple": true`, n8n parses the email and only returns a clean JSON structure (subject, text, from, id), which is vastly more performant and token-efficient for LLMs.
**Action:** Changed `"simple": false` to `"simple": true` in the `n8n-nodes-base.gmailTool` nodes to optimize payloads sent to the LLM.
