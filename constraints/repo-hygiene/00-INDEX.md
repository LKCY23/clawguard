# systems/constraints/constraints/repo-hygiene/00-INDEX.md

Pack:
- repo-hygiene

Purpose:
- Provide routing + compliance checks for workspace persistence and git operations.

Triggers (examples):
- Any write/create/edit/move under the workspace.
- Git operations: add/commit/push/branch/rebase/submodule changes.

Minimal reads:
- MUST load the repo hygiene rules pack entrypoint: `systems/constraints/rules/repo-hygiene/00-INDEX.md`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation (reading systems/constraints/rules/constraints outside this pack) requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Authorization (write/commit/push; shared vs main): `systems/constraints/rules/repo-hygiene/active/REPO-010-write-and-commit-authorization.md`
- Submodules under `repositories/`: `systems/constraints/rules/repo-hygiene/active/20-repositories-are-submodules.md`

Checks (reference-first):
- Verify applicable constraints in `systems/constraints/rules/repo-hygiene/active/REPO-010-write-and-commit-authorization.md` before any durable action.
- Persistent local state changes MUST satisfy the applicable pre-action self-check / receipt rule before execution.
- If governance files changed, enforce `systems/constraints/rules/repo-hygiene/active/REPO-030-governance-lint-gate.md` before commit/push.
- MUST avoid destructive git operations unless explicitly requested.
- MUST avoid reverting unrelated user changes; keep diffs scoped.
- Reporting receipt (include in the user-visible response for high-risk repo actions):
  - Applied rules: REPO-010[, REPO-030][, REPO-020]
  - Checks performed: authorization gate for write/commit/push (REPO-010); governance lint gate when governance files changed (REPO-030); submodule policy when touching repositories/ (REPO-020)

Authoritative references:
- `systems/constraints/rules/repo-hygiene/00-INDEX.md`
- `systems/constraints/registry/repo.yml`
