Title: Registry Must Route to Constraints Entrypoints

ID: GOV-040
Status: active
Pack: rules-governance
Owner: user
Last Updated: 2026-03-04

When:
- Any time the agent creates or edits `systems/constraints/registry/*.yml` entries.

Rule:
- Each registry entry MUST include:
  - `domain: <domain>`
  - `pack: <pack>`
  - `minimalReadMain` including `systems/constraints/constraints/<pack>/00-INDEX.md`
  - `minimalReadShared` including `systems/constraints/constraints/<pack>/00-INDEX.md`
- Registry entries MUST NOT route directly to `systems/constraints/rules/<pack>/*` as their minimal reads.

Rationale:
- Registry is a routing layer. Constraints are the pack entrypoints that define minimal/escalation reads into rules.

Examples:
- Correct minimal reads:
  - `minimalReadMain: [constraints/exec-tool/00-INDEX.md]`
- Incorrect minimal reads:
  - `minimalReadMain: [rules/exec-tool/00-INDEX.md]`

Failure Modes:
- Registry points at rules directly -> duplication, drift, and bypassing constraints checks.

Related:
- `systems/constraints/registry/00-INDEX.md`
- `systems/constraints/constraints/<pack>/00-INDEX.md`
