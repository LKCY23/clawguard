# systems/constraints/rules/rules-governance/00-INDEX.md

Purpose:
- Define governance rules for the local rules system and the on-demand registry workflow (registration, discoverability).
- Define the constitution-level conflict resolution order via `systems/constraints/CONSTRAINTS_SYSTEM.md` for the full constraints system (systems/constraints/registry/constraints/rules/skills).

Non-goals / Out of scope:
- `AGENTS.md` operator playbooks and personal workflow preferences (these are not normative).
- Domain-specific operational rules (those belong in their own packs).

Entry Decision Tree (navigation-first):
- Did you create a new rules pack or activate a rule?
  - Follow GOV-020 (ensure README discoverability + registry registration).

Terms:
- `systems/constraints/CONSTRAINTS_SYSTEM.md`: constitution-level conflict resolution and lifecycle governance across all packs and router layers.
- pack: a rules domain folder under `systems/constraints/rules/<pack>/`.
- register: add a `systems/constraints/registry/<pack>.yml` entry and list it in `systems/constraints/registry/00-INDEX.md`.

Decision Order (within this pack):
0) `systems/constraints/CONSTRAINTS_SYSTEM.md` conflict resolution order (constitution-level; applies across all packs and layers)
1) Correctness (packs with Active Rules must be registered)
2) Reproducibility (stable paths, least-read minimalRead lists)
3) Maintainability (thin indexes, small entries)

Active Rules (default-enforced):
- [GOV-020] Auto-Register When Rules Become Active — Packs with active rules MUST have registry entries
- [GOV-030] Constraints Skill Maintenance — Keep `skills/constraints/SKILL.md` consistent with systems/constraints/registry/pack wiring
- [GOV-040] Registry Must Route to Constraints Entrypoints — Registry minimal reads must point to `systems/constraints/constraints/<pack>/00-INDEX.md`
- [GOV-050] Governance Lint Gate — Run governance lint after governance edits before commit/push
- [GOV-060] Runtime Receipts for High-Risk Packs — Applied rules (and optional checks performed) receipts
- [GOV-070] Update Governance When Runtime Constraints System Structure Changes — Notify and align governance artifacts
- [GOV-080] Gitignore Tracking Hygiene — When ignore rules change, untrack now-ignored files explicitly

Verification:
- Run: `node scripts/lint-governance.mjs`
