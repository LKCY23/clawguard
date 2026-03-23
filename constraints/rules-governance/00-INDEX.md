# systems/constraints/constraints/rules-governance/00-INDEX.md

Pack:
- rules-governance

Purpose:
- Provide routing + compliance checks for maintaining the local rules system (packs, registry entries, lifecycle metadata).

Triggers (examples):
- User asks to add/update/remove rules.
- User asks about rule conflicts, pack overrides, governance, or registry wiring.

Minimal reads:
- MUST load the global governance framework: `systems/constraints/CONSTRAINTS_SYSTEM.md`.
- MUST load the governance rules pack entrypoint: `systems/constraints/rules/rules-governance/00-INDEX.md`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation (reading systems/constraints/rules/constraints outside this pack) requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Pack registration or active rule lifecycle changes: `systems/constraints/rules/rules-governance/active/20-auto-register-on-rule-changes.md`
- Constraints skill + runtime routing changes: `systems/constraints/rules/rules-governance/active/GOV-030-constraints-skill-maintenance.md`
- Registry -> constraints routing requirements: `systems/constraints/rules/rules-governance/active/GOV-040-registry-must-route-to-constraints.md`
- Governance lint gate: `systems/constraints/rules/rules-governance/active/GOV-050-governance-lint-gate.md`
- Runtime receipts: `systems/constraints/rules/rules-governance/active/GOV-060-runtime-receipts-high-risk.md`
- System structure changes: `systems/constraints/rules/rules-governance/active/GOV-070-governance-update-on-system-structure-change.md`

Checks (reference-first):
- Verify applicable constraints in `systems/constraints/rules/rules-governance/active/20-auto-register-on-rule-changes.md` before editing packs/registry entries.
- If governance files change, enforce `systems/constraints/rules/rules-governance/active/GOV-050-governance-lint-gate.md`.
- Reporting receipt (include in the user-visible response when governance files change):
  - Applied rules: GOV-020[, GOV-030][, GOV-040][, GOV-050][, GOV-060][, GOV-070]
  - Checks performed: registration updated for active packs (GOV-020); constraints skill consistency when applicable (GOV-030); registry routes to constraints (GOV-040); governance lint OK/FAIL (GOV-050); receipts policy (GOV-060); governance updated on system structure changes when applicable (GOV-070)

Authoritative references:
- `systems/constraints/CONSTRAINTS_SYSTEM.md`
- `systems/constraints/rules/rules-governance/00-INDEX.md`
- `systems/constraints/registry/rules-governance.yml`

Verification:
- Run: `node scripts/lint-governance.mjs`
