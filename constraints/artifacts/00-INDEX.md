# systems/constraints/constraints/artifacts/00-INDEX.md

Pack:
- artifacts

Purpose:
- Provide routing + compliance checks for temporary artifacts created during agent work (snapshots, exports, generated files).

Triggers (examples):
- User asks to export, generate, snapshot, or produce files.
- Agent is about to create temporary outputs for debugging or research.

Minimal reads:
- MUST load the artifacts rules pack entrypoint: `systems/constraints/rules/artifacts/00-INDEX.md`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation (reading systems/constraints/rules/constraints outside this pack) requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Snapshots (storage + lifecycle): `systems/constraints/rules/artifacts/active/10-snapshots-lifecycle.md`
- Generated outputs (retention): `systems/constraints/rules/artifacts/active/20-generated-retention.md`

Checks (reference-first):
- Verify applicable constraints in `systems/constraints/rules/artifacts/active/10-snapshots-lifecycle.md` before creating snapshots.
- Verify applicable constraints in `systems/constraints/rules/artifacts/active/20-generated-retention.md` before storing generated outputs.
- Reporting receipt (include in the user-visible response when artifacts are created/retained):
  - Applied rules: ART-010[, ART-020]
  - Checks performed: snapshot location/retention gate (ART-010) when creating snapshots; generated retention/TTL gate (ART-020) when storing outputs

Authoritative references:
- `systems/constraints/rules/artifacts/00-INDEX.md`
- `systems/constraints/registry/artifacts.yml`
