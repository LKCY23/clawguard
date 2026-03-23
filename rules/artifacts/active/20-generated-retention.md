Title: Generated Artifacts Storage and Retention

ID: ART-020
Status: active
Pack: artifacts
Owner: user
Last Updated: 2026-03-02

When:
- The agent creates generated outputs as a byproduct of work (exports, build outputs, render results, intermediate files).

Rule:
- MUST store generated outputs under `/Users/openclaw/.openclaw/workspace/artifacts/generated/`.
- MUST NOT store primary/source-of-truth files under `artifacts/generated/`.
- SHOULD group outputs by task or project using a subdirectory (recommended: `artifacts/generated/<task-or-project>/`).
- SHOULD use a timestamped or versioned filename to avoid accidental overwrites.
- MUST delete generated outputs once they are no longer needed to complete the current task.
- MUST delete any generated output older than 7 days.
- MAY opt out specific subdirectories from TTL deletion by placing a `.keep` file in that directory.

Rationale:
- Generated outputs are usually reproducible; keeping them forever increases clutter and risk.
- A single directory + predictable retention makes cleanup safe and automatable.

Examples:
- Export: write `artifacts/generated/agent-governance-ppt/export-20260302-001829.pdf`.
- Task grouping: write all outputs for a task under `artifacts/generated/issue-123/`.
- Keep: create `artifacts/generated/agent-governance-ppt/.keep` to preserve that directory.

Failure Modes:
- Putting source files in generated -> move them into a proper project directory under `workspace/`.
- Important output deleted by TTL -> add a `.keep` sentinel or move it to a non-artifact location.

Related:
- `systems/constraints/CONSTRAINTS_SYSTEM.md`
- `systems/constraints/rules/artifacts/00-INDEX.md`
- `systems/constraints/rules/artifacts/draft/10-snapshots-lifecycle.md`
