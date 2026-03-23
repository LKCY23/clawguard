# systems/constraints/constraints/ssh-host-admin/00-INDEX.md

Pack:
- ssh-host-admin

Purpose:
- Provide routing + compliance checks for SSH access to the host terminal.
- Treat host-shell SSH as a distinct, higher-risk control surface than ordinary local exec.

Triggers (examples):
- User asks the agent to SSH into the host machine.
- User asks the agent to use `ssh host-docker` or otherwise open a host shell.
- Agent is about to use host-terminal SSH to inspect or change Docker- or service-related state.

Minimal reads:
- MUST load the ssh-host-admin rules pack entrypoint: `systems/constraints/rules/ssh-host-admin/00-INDEX.md`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Allowed SSH targets: `systems/constraints/rules/ssh-host-admin/active/SSHHOST-010-host-target-allowlist.md`
- Scope limits: `systems/constraints/rules/ssh-host-admin/active/SSHHOST-020-scope-limited-to-docker-and-service-paths.md`
- Destructive boundaries: `systems/constraints/rules/ssh-host-admin/active/SSHHOST-030-no-destructive-host-operations.md`
- Authorization gate: `systems/constraints/rules/ssh-host-admin/active/SSHHOST-040-host-terminal-requires-explicit-authorization.md`
- Preflight/reporting: `systems/constraints/rules/ssh-host-admin/active/SSHHOST-050-preflight-and-command-reporting.md`

Checks (reference-first):
- MUST verify the SSH target is explicitly allowed before connecting.
- MUST verify the user has explicitly authorized host-terminal SSH use for this action before connecting.
- MUST verify the intended scope is limited to Docker / target-service-related inspection or modification only.
- MUST NOT perform destructive host operations unless separately and explicitly authorized.
- MUST provide a pre-action self-check / receipt before using the host terminal.
- Reporting receipt (include in the user-visible response for host-terminal SSH actions):
  - Applied rules: SSHHOST-010[, SSHHOST-020][, SSHHOST-030][, SSHHOST-040][, SSHHOST-050]
  - Checks performed: allowed target verified (SSHHOST-010); scope verified (SSHHOST-020); destructive boundary checked (SSHHOST-030); explicit authorization verified (SSHHOST-040); preflight/reporting completed (SSHHOST-050)

Authoritative references:
- `systems/constraints/rules/ssh-host-admin/00-INDEX.md`
- `systems/constraints/registry/ssh-host-admin.yml`
