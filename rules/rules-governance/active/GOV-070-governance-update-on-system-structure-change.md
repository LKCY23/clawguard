Title: Update Governance When Runtime Constraints System Structure Changes

ID: GOV-070
Status: active
Pack: rules-governance
Owner: user
Last Updated: 2026-03-04

When:
- Any time the runtime constraints system structure changes, including:
  - adding/removing/renaming a component module (systems/constraints/registry/constraints/rules/skills/scripts/hooks)
  - adding/removing a new pack/domain
  - changing the routing chain (registry -> constraints -> rules) or adding a new entrypoint

Rule:
- MUST notify the user that `rules-governance` may require updates.
- MUST review and update governance artifacts as needed:
  - `systems/constraints/rules/rules-governance/00-INDEX.md` (Active Rules list)
  - `systems/constraints/constraints/rules-governance/00-INDEX.md` (escalation reads, checks, receipts)
  - `scripts/lint-governance.mjs` (coverage of required entrypoints)
  - If commit/push gates are affected: `systems/constraints/rules/repo-hygiene/active/REPO-030-governance-lint-gate.md` and local git hooks
- MUST run `node scripts/lint-governance.mjs` after changes.

Rationale:
- Governance rules are the system's meta-contract. When the system changes shape, governance must stay aligned to avoid drift.

Examples:
- Adding a new component under `skills/` for routing -> update GOV rules and expand lint to validate the new entrypoint.
- Adding a new pack `messaging` -> add `systems/constraints/registry/messaging.yml`, `systems/constraints/constraints/messaging/00-INDEX.md`, and update governance checks.

Failure Modes:
- New component added but governance not updated -> missing gate; drift accumulates.

Related:
- [GOV-050] Governance Lint Gate
- `systems/constraints/rules/rules-governance/00-INDEX.md`
- `systems/constraints/constraints/rules-governance/00-INDEX.md`
- `scripts/lint-governance.mjs`
