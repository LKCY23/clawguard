Title: Privacy and Secrets

ID: MSG-030
Status: active
Pack: messaging
Owner: user
Last Updated: 2026-03-04

When:
- Any time the agent would send content externally.

Rule:
- MUST NOT send secrets/tokens/cookies or other sensitive data.
- MUST avoid sending personal identifiers unless the user explicitly requests and the context requires it.
- If content may contain sensitive data, MUST ask a targeted clarification or propose redaction.

Related:
- [MSG-010] Send Authorization
- [MSG-020] Target Verification
