
## 2024-08-28 - Conversational UX formatting for headless agents
**Learning:** When dealing with headless AI agents that communicate via messaging platforms (like Telegram or WhatsApp), traditional frontend UX rules (ARIA, focus states) don't apply. However, micro-UX improvements can still be achieved by enforcing strict output formatting rules (bold headers, bullet points, monospace for technical data) in the agent's core system prompt.
**Action:** Apply formatting constraints to system prompts to enhance message hierarchy and readability on conversational surfaces.

## 2024-11-28 - Emojis as visual anchors for headless agents
**Learning:** For text-based interaction models like messaging platforms, providing visual structure using emojis acts as a great micro-UX improvement to aid scanning and readability in AI-generated messages.
**Action:** Enhance conversational formatting rules in system prompts to include the instruction to use emojis as visual anchors.

## 2024-12-01 - Native message threading for better conversational context
**Learning:** In asynchronous headless conversational AI (like Telegram bots), users can lose context if the AI takes time to respond or if the user sends multiple messages. Using native platform features like direct message replies (`reply_to_message_id`) creates a visual thread, significantly improving the interaction model and contextual clarity.
**Action:** Always use native message threading/reply features for outbound messages when responding to a specific user input in messaging workflows.

## 2024-12-01 - Native message threading for WhatsApp conversational context
**Learning:** In asynchronous headless conversational AI (like WhatsApp bots), users can lose context if the AI takes time to respond or if the user sends multiple messages. Using native platform features like direct message replies (`quoted.messageQuoted.messageId` in Evolution API) creates a visual thread, significantly improving the interaction model and contextual clarity.
**Action:** Always use native message threading/reply features for outbound messages when responding to a specific user input in WhatsApp messaging workflows.

## 2024-12-05 - Immediate visual feedback for high-latency operations
**Learning:** For headless conversational AI agents performing complex operations (like LLM interactions and multiple tool calls), the lack of immediate feedback can make the application feel unresponsive. Sending a native "Typing..." action immediately after receiving user input is a crucial micro-UX improvement that manages user expectations and perceived latency.
**Action:** Always insert a "Typing" or processing indicator node immediately following input triggers before routing to high-latency LLM or processing nodes.

## 2025-01-20 - Resilient micro-UX for conversational surfaces
**Learning:** Adding auxiliary micro-UX enhancements (like typing indicators) shouldn't risk breaking the core conversational flow if the underlying platform API fails. By explicitly telling the workflow engine to continue on failure for these non-critical nodes, we achieve resilient micro-UX that degrades gracefully without crashing the main user journey.
**Action:** Always configure non-critical auxiliary UX nodes (such as 'Typing Indicators' or presence updates) with `"continueOnFail": true` in the workflow structure.
