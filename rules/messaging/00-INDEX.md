# systems/constraints/rules/messaging/00-INDEX.md

Purpose:
- Define the normative rules for outbound messaging actions (sending, replying, editing, deleting).

Scope:
- Applies whenever the agent communicates externally via provider channels (Feishu/Telegram/etc).

Decision order:
- Authorization -> Target -> Privacy -> Representation -> Send -> Reporting.

Active Rules (default-enforced):
- [MSG-010] Send Authorization — Do not send unless explicitly authorized; default to draft
- [MSG-020] Target Verification — Confirm channel/recipient; avoid wrong-target sends
- [MSG-030] Privacy and Secrets — Prevent sending sensitive content externally
- [MSG-040] Representation and Tone — Avoid speaking as the user without confirmation
- [MSG-050] Reporting — Include Applied rules receipt and delivery details
- [MSG-060] Bootstrap Any Files Notify Workflow — Opportunistic helper-driven notify with idempotent dedupe
