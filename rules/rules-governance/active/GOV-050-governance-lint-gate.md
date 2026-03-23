Title: Governance Lint Gate

ID: GOV-050
Status: active
Pack: rules-governance
Owner: user
Last Updated: 2026-03-04

When:
- Any time governance files are changed:
  - `systems/constraints/registry/`
  - `systems/constraints/constraints/`
  - `systems/constraints/rules/`
  - `skills/`
  - `systems/constraints/CONSTRAINTS.md`
  - `systems/constraints/CONSTRAINTS_SYSTEM.md`
  - `scripts/lint-governance.mjs`

Rule:
- MUST run `node scripts/lint-governance.mjs` after governance changes and before committing/pushing.
- MUST report the lint result in the user-visible response when governance files change:
  - "Governance lint: OK" OR
  - "Governance lint: FAIL" + the first relevant error lines

Rationale:
- Governance edits can silently break routing (registry -> constraints -> rules). A deterministic lint gate prevents drift.

Examples:
- After editing a registry entry:
  - `node scripts/lint-governance.mjs`

Failure Modes:
- Lint fails but changes are committed -> later runs miss constraints or read missing paths.

Related:
- `scripts/lint-governance.mjs`
- `scripts/install-git-hooks.sh`
- [REPO-030] Governance Lint Gate
