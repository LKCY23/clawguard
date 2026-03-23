Title: OpenClaw Update Checks

ID: MAINT-010
Status: active
Pack: maintenance
Owner: user
Last Updated: 2026-03-02

When:
- The user asks for periodic update checks for OpenClaw.
- The agent is running on a machine where `openclaw` is installed.

Rule:
- MUST check for OpenClaw updates at most once per 24 hours.
- MUST remind the user when an update is available.
- MUST include in the reminder:
  - installed version
  - latest available version (source used)
  - one safe update command suggestion (but do not run it)
  - a note that if the OpenClaw Gateway is running, a restart is required for the new version to take effect
  - a short community signal check (issues/discussions) for the target version when feasible
- MUST NOT upgrade OpenClaw without explicit user instruction.
- MUST NOT restart the OpenClaw Gateway without explicit user instruction.
- SHOULD avoid noisy reminders when there is no update.
- SHOULD consult `systems/constraints/rules/web-search/00-INDEX.md` when doing community signal checks.
- MAY include a short link to relevant changelog/release notes when available.

Rationale:
- Version drift causes confusing behavior; reminders keep the user informed.
- Upgrades can change behavior; user agency is required.

Examples:
- Reminder (update available): "OpenClaw installed: 2026.2.26; latest: 2026.3.2. To update (run only if you decide to upgrade): `npm i -g openclaw@2026.3.2`. If the Gateway is running, restart is required for the new version to take effect (I will not restart it unless you ask)."
- Reminder (no update): say nothing unless the user asks.

Failure Modes:
- Agent upgrades automatically -> treat as a bug; stop and ask for explicit instruction.
- Version source ambiguous -> include the source (npm registry vs GitHub).

Related:
- `systems/constraints/CONSTRAINTS_SYSTEM.md`
- `systems/constraints/rules/repo-hygiene/active/10-write-and-commit-authorization.md` (commit/push authorization)
