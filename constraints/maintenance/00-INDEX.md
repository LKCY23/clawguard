# systems/constraints/constraints/maintenance/00-INDEX.md

Pack:
- maintenance

Purpose:
- Provide routing + compliance checks for maintenance/update-related actions.

Triggers (examples):
- User asks to update OpenClaw or run maintenance checks.
- Agent performs periodic update checks.

Minimal reads:
- MUST load the maintenance rules pack entrypoint: `systems/constraints/rules/maintenance/00-INDEX.md`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation (reading systems/constraints/rules/constraints outside this pack) requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Update check vs upgrade behavior: `systems/constraints/rules/maintenance/active/10-openclaw-update-checks.md`
- Gateway restart handshake: `systems/constraints/rules/maintenance/active/MAINT-020-gateway-restart-handshake.md`

Checks (reference-first):
- Verify applicable constraints in `systems/constraints/rules/maintenance/active/10-openclaw-update-checks.md` before running maintenance/update actions.
- If about to restart the gateway, enforce `systems/constraints/rules/maintenance/active/MAINT-020-gateway-restart-handshake.md`.
- Reporting receipt (include in the user-visible response when maintenance actions run):
  - Applied rules: MAINT-010[, MAINT-020]
  - Checks performed: no auto-upgrade; update check vs upgrade boundary; post-change status verification when applicable (MAINT-010); restart handshake notices when applicable (MAINT-020)

Authoritative references:
- `systems/constraints/rules/maintenance/00-INDEX.md`
- `systems/constraints/registry/maintenance.yml`
