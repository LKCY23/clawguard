Title: Constraints Skill Maintenance

ID: GOV-030
Status: active
Pack: rules-governance
Owner: user
Last Updated: 2026-03-04

When:
- Any time the agent changes the runtime governance system components:
  - `systems/constraints/registry/*.yml`
  - `systems/constraints/constraints/<pack>/00-INDEX.md`
  - `systems/constraints/rules/<pack>/*`
  - `skills/constraints/SKILL.md`

Rule:
- MUST treat `skills/constraints/SKILL.md` as part of the runtime governance system.
- When adding/removing/renaming a registry entry or changing a registry entry's `pack:` mapping:
  - MUST check whether `skills/constraints/SKILL.md` needs an update to stay consistent.
- MUST note that skill changes take effect on a new session (skills snapshot); do not assume mid-session behavior changes.

Rationale:
- The constraints skill teaches per-run routing and pack application. If systems/constraints/registry/pack wiring changes, the skill must not drift.

Examples:
- Add a new registry entry `systems/constraints/registry/messaging.yml` (pack: messaging) -> update `skills/constraints/SKILL.md` routing guidance.

Failure Modes:
- Registry changed but skill not updated -> agent misses packs or routes incorrectly.

Related:
- [GOV-020] Auto-Register When Rules Become Active
- `skills/constraints/SKILL.md`
- `systems/constraints/registry/00-INDEX.md`
