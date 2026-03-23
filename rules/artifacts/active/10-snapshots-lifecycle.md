Title: Snapshot Storage and Lifecycle

ID: ART-010
Status: active
Pack: artifacts
Owner: user
Last Updated: 2026-03-02

When:
- The agent creates or moves screenshots/snapshots during interactive work.
- The agent needs to persist temporary visual artifacts for troubleshooting or UI automation.

Rule:
- MUST store all screenshots under `/Users/openclaw/.openclaw/workspace/artifacts/snapshots/`.
- MUST use a timestamped filename (recommended: `<prefix>-YYYYMMDD-HHMMSS.png`) to avoid collisions.
- MUST delete snapshots once they are no longer needed to complete the current task.
- MUST delete any snapshot file older than 48 hours.
- MUST store snapshots only under `artifacts/snapshots/` and MUST NOT store them under `artifacts/generated/`.
- SHOULD run a garbage-collection pass at the end of tasks that created snapshots.
- MAY run periodic garbage collection (e.g., hourly) to enforce TTL.

Rationale:
- Snapshots are temporary artifacts; keeping them indefinitely creates clutter and privacy risk.
- A single directory and deterministic naming make cleanup and retrieval predictable.

Examples:
- Create: save `login-20260302-001533.png` to `artifacts/snapshots/`.
- End-of-task cleanup: delete snapshots created for that task once the task is complete.
- TTL cleanup: delete any `*.png` in `artifacts/snapshots/` with mtime older than 48h.

Failure Modes:
- Snapshots saved elsewhere -> move them into `artifacts/snapshots/` and delete originals.
- Forgetting cleanup after task -> run GC and remove task-specific files.

Related:
- `systems/constraints/CONSTRAINTS_SYSTEM.md`
- `systems/constraints/rules/artifacts/00-INDEX.md`
