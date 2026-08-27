## 2024-03-01 - Authorization Bypass in n8n Filters
**Vulnerability:** A filter node designed for authentication was using `$if` to compare the incoming `chat.id` with the same `chat.id` retrieved dynamically via `$('Telegram Trigger').item.json.message.chat.id`. This effectively resulted in `if (incoming.chat.id == incoming.chat.id)`, making the condition always true.
**Learning:** In visual no-code workflows, it is easy to accidentally link an authorization verification check back to the untrusted input itself, instead of comparing it to a known static secret or environment variable.
**Prevention:** Hardcode or strictly reference environment variables for secrets/IDs (`=YOUR_TELEGRAM_CHAT_ID`) instead of writing complex dynamic fallbacks in filter conditions.

## 2024-05-24 - Defensive Limits and Type Casting in n8n Data Operations
**Vulnerability:** Unbounded data fetches and potential type coercion risks in n8n workflows (especially Postgres, Gmail, and Data Table nodes) leading to memory bloat, latency spikes, and context window overflows in LLM agent workflows. Missing explicit cast for parameters representing IDs in Postgres nodes.
**Learning:** In n8n LLM agent workflows, data fetching nodes can return excessive amounts of data if `returnAll` is true or if limits are absent, overwhelming the system and LLM context. Type coercion in parameterized Postgres queries can lead to unexpected errors or security issues if IDs are not explicitly cast.
**Prevention:** Always implement defensive limits (e.g., `limit: 50` and `returnAll: false`) on tool and database nodes. When writing parameterized SQL queries for n8n PostgreSQL nodes, explicitly cast parameters representing IDs or integers (e.g., `$1::int` or `$1::bigint`).
