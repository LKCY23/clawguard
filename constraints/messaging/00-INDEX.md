# systems/constraints/constraints/messaging/00-INDEX.md

Pack:
- messaging

Purpose:
- Provide routing + compliance checks for outbound messaging actions.

Triggers (examples):
- User asks the agent to send/reply/notify in an external channel (Feishu/Telegram/etc).
- Agent is about to call `message` tool (send/edit/delete/react/broadcast).

Minimal reads:
- MUST load the messaging rules pack entrypoint: `systems/constraints/rules/messaging/00-INDEX.md`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation (reading systems/constraints/rules/constraints outside this pack) requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Authorization gate: `systems/constraints/rules/messaging/active/MSG-010-send-authorization.md`
- Target verification: `systems/constraints/rules/messaging/active/MSG-020-target-verification.md`
- Privacy/secrets check: `systems/constraints/rules/messaging/active/MSG-030-privacy-and-secrets.md`
- Representation and tone: `systems/constraints/rules/messaging/active/MSG-040-representation-and-tone.md`
- Messaging reporting: `systems/constraints/rules/messaging/active/MSG-050-reporting.md`

Checks (reference-first):
- Before sending/editing/deleting/reacting/broadcasting, verify applicable constraints in:
  - `systems/constraints/rules/messaging/active/MSG-010-send-authorization.md`
  - `systems/constraints/rules/messaging/active/MSG-020-target-verification.md`
  - `systems/constraints/rules/messaging/active/MSG-030-privacy-and-secrets.md`
- Reporting receipt (include in the user-visible response for high-risk messaging actions):
  - Applied rules: MSG-010[, MSG-020][, MSG-030][, MSG-040][, MSG-050]
  - Checks performed: send authorization confirmed (MSG-010); target verified (MSG-020); privacy scan (MSG-030); representation clarified (MSG-040) when applicable

Authoritative references:
- `systems/constraints/rules/messaging/00-INDEX.md`
- `systems/constraints/registry/messaging.yml`
