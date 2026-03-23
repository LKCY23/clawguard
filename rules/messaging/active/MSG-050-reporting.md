Title: Messaging Reporting

ID: MSG-050
Status: active
Pack: messaging
Owner: user
Last Updated: 2026-03-04

When:
- Any time the agent sends/edits/deletes/reacts/broadcasts a message.

Rule:
- MUST include an "Applied rules" line listing the messaging rule IDs followed (e.g., MSG-010, MSG-020, MSG-030).
- MUST provide a concise confirmation including:
  - destination (channel/provider + recipient/chat name/id when available)
  - messageId when available
- SHOULD avoid pasting sensitive full message content; summarize if needed.
