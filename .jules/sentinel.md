## 2024-03-01 - Authorization Bypass in n8n Filters
**Vulnerability:** A filter node designed for authentication was using `$if` to compare the incoming `chat.id` with the same `chat.id` retrieved dynamically via `$('Telegram Trigger').item.json.message.chat.id`. This effectively resulted in `if (incoming.chat.id == incoming.chat.id)`, making the condition always true.
**Learning:** In visual no-code workflows, it is easy to accidentally link an authorization verification check back to the untrusted input itself, instead of comparing it to a known static secret or environment variable.
**Prevention:** Hardcode or strictly reference environment variables for secrets/IDs (`=YOUR_TELEGRAM_CHAT_ID`) instead of writing complex dynamic fallbacks in filter conditions.

## Security Vulnerability: SQL Injection via String Interpolation in n8n Postgres Nodes

### 🎯 The Vulnerability
In `n8nClaw.json`, a PostgreSQL node was executing a query that used direct string interpolation to inject dynamic variables into the SQL statement. The vulnerable query pattern was:
```sql
SELECT session_id, message, id
FROM n8n_chat_histories
WHERE id > '{{ $json.last_vector_id }}'
ORDER BY id ASC
```

### ⚠️ The Risk
Directly interpolating values like `{{ $json.last_vector_id }}` allows an attacker to inject arbitrary SQL statements if they can control or manipulate the `last_vector_id` value. This could lead to data exfiltration, database manipulation, or privilege escalation.

### 🛡️ The Solution
To fix this vulnerability, we parameterize the SQL query and pass the dynamic values through the `queryParameters` property. Furthermore, to prevent type coercion attacks, we explicitly cast the parameterized variable (e.g., to an integer) directly within the SQL statement.

The fixed query pattern:
```json
"query": "SELECT session_id, message, id\nFROM n8n_chat_histories \nWHERE id > $1::int\nORDER BY id ASC\n",
"options": {
  "queryParameters": "{{ $json.last_vector_id }}"
}
```

### 📋 Guidelines for Future Development
1. **Never** use string interpolation (e.g., `{{ ... }}`) directly inside a SQL query string.
2. **Always** use parameterized variables (e.g., `$1`, `$2`) in the SQL statement.
3. Pass the actual dynamic values using the node's `queryParameters` option.
4. Add **explicit casting** to parameters in the SQL (e.g., `$1::int`, `$1::uuid`) to enforce type safety and avoid unexpected database behavior.
