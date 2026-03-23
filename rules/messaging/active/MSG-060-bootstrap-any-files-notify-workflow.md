# systems/constraints/rules/messaging/active/MSG-060-bootstrap-any-files-notify-workflow.md

Title: Bootstrap Any Files Notify Workflow

ID: MSG-060
Status: active
Pack: messaging
Owner: user
Last Updated: 2026-03-05

When:
- In MAIN SESSION, when the agent observes that `bootstrap-any-files` injection state has been produced or updated, typically via `workspace/tmp/bootstrap-any-files-state.json`.

Rule:
- The agent SHOULD run the notify helper (`workspace/skills/notify/run/bootstrap-any-files.py`) opportunistically after workflows that can produce a new injection state (e.g., gateway restart / new agent run).
- If helper output indicates `shouldSend=true`, the agent MUST:
  - Send the injection summary message to the fixed targets (Feishu + Telegram).
  - Append the helper `fingerprint` to `workspace/tmp/bootstrap-any-files-state.sent` after successful sends.
- The agent MUST NOT send if helper indicates `reason=no-state` or `reason=already-sent`.

Rationale:
- OpenClaw currently lacks a stable per-turn mandatory hook to enforce notify automatically.
- This workflow makes notify effectively automatic without relying on cron or restart-triggered one-time throttling.

Security:
- Only send aggregated stats (added/total/truncated/version). Do not include file paths or contents.

Related:
- [MSG-010] Send Authorization
- [MSG-020] Target Verification
- [MSG-030] No Sensitive External Disclosure
