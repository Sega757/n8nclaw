## 2024-03-01 - Authorization Bypass in n8n Filters
**Vulnerability:** A filter node designed for authentication was using `$if` to compare the incoming `chat.id` with the same `chat.id` retrieved dynamically via `$('Telegram Trigger').item.json.message.chat.id`. This effectively resulted in `if (incoming.chat.id == incoming.chat.id)`, making the condition always true.
**Learning:** In visual no-code workflows, it is easy to accidentally link an authorization verification check back to the untrusted input itself, instead of comparing it to a known static secret or environment variable.
**Prevention:** Hardcode or strictly reference environment variables for secrets/IDs (`=YOUR_TELEGRAM_CHAT_ID`) instead of writing complex dynamic fallbacks in filter conditions.
