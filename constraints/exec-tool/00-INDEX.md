# systems/constraints/constraints/exec-tool/00-INDEX.md

Pack:
- exec-tool

Purpose:
- Provide routing + compliance checks for shell execution actions.

Triggers (examples):
- Any use of `functions.exec` / shell / terminal commands.
- Any instruction that could modify/delete files, change network/security posture, or impact accounts.

Minimal reads:
- MUST load the exec rules pack entrypoint: `systems/constraints/rules/exec-tool/00-INDEX.md`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation (reading systems/constraints/rules/constraints outside this pack) requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Safety and destructive actions: `systems/constraints/rules/exec-tool/active/EXEC-060-exec-safety.md`
- Long-running / PTY / background: `systems/constraints/rules/exec-tool/active/EXEC-050-long-running-and-pty.md`
- Where to run (host/sandbox/node): `systems/constraints/rules/exec-tool/active/EXEC-030-where-to-run.md`
- Wrapper choice: `systems/constraints/rules/exec-tool/active/EXEC-010-wrapper-selection.md`
- PATH / binary resolution: `systems/constraints/rules/exec-tool/active/EXEC-020-path-and-binary-resolution.md`
- Posture/approvals: `systems/constraints/rules/exec-tool/active/EXEC-025-exec-host-security-approvals.md`

Checks (reference-first):
- Verify applicable constraints in `systems/constraints/rules/exec-tool/active/EXEC-060-exec-safety.md` before running any command.
- Stateful exec actions (config edits, daemon/service lifecycle changes, runtime-affecting local mutations) MUST satisfy the applicable pre-action self-check / receipt rule before execution.
- If the user request is destructive/irreversible or intent is ambiguous, MUST ask one targeted clarification.
- Reporting receipt (include in the user-visible response for high-risk exec actions):
  - Applied rules: EXEC-060[, EXEC-050][, EXEC-030][, EXEC-010][, EXEC-020][, EXEC-025]
  - Checks performed: destructive/irreversible gate (EXEC-060); long-running/PTY strategy (EXEC-050) when applicable; target selection (EXEC-030) when applicable

Authoritative references:
- `systems/constraints/rules/exec-tool/00-INDEX.md`
- `systems/constraints/registry/exec.yml`
