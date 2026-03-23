Title: Send Authorization

ID: MSG-010
Status: active
Pack: messaging
Owner: user
Last Updated: 2026-03-04

When:
- Any time the agent is asked to send a message in an external channel.

Rule:
- MUST NOT send a message unless the user explicitly requests sending.
- Default behavior SHOULD be to provide a draft for user confirmation.
- If the user explicitly says to send without review (e.g., "直接发"), MAY send after completing privacy/target checks.

Examples:
- "帮我回他" -> draft + ask to confirm send.
- "直接发给他" -> proceed after checks.

Related:
- [MSG-020] Target Verification
- [MSG-030] Privacy and Secrets
