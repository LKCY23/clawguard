# systems/constraints/rules/artifacts/00-INDEX.md

Purpose:
- Define rules for temporary artifacts created during agent work (e.g., screenshots, generated outputs).

Non-goals / Out of scope:
- How to use `functions.exec` or specific tools (see `systems/constraints/rules/exec-tool/`).
- Git commit/push policy (see `systems/constraints/rules/repo-hygiene/`).

Entry Decision Tree (navigation-first):
- Did the task create temporary files (screenshots, exports, generated files)?
  - Yes: store them only under `artifacts/`.
  - If no longer needed: delete immediately.
  - If still needed: ensure TTL cleanup applies.

Terms:
- artifact: a file/directory created as a byproduct of agent work, not the primary source of truth.
- snapshots: screenshots and UI captures.
- generated: generated outputs (exports, build artifacts, render outputs).
- TTL: time-to-live; max retention window before deletion.

Decision Order (within this pack):
1) Privacy and safety (minimize retention; delete ASAP)
2) Correctness (retain only while needed to complete the task)
3) Reproducibility (deterministic paths/names for short-lived artifacts)
4) Convenience (easy to find when needed)

Active Rules (default-enforced):
- [ART-010] Snapshot Storage and Lifecycle — Store snapshots in one directory and delete when done / after TTL
- [ART-020] Generated Artifacts Storage and Retention — Keep generated outputs scoped and prunable (7d TTL, optional .keep)

Draft Rules (not default):
- (none)
