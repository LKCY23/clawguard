Title: Governance Lint Gate

ID: REPO-030
Status: active
Pack: repo-hygiene
Owner: user
Last Updated: 2026-03-04

When:
- Any time the agent is asked to commit or push changes.
- Any time the agent modifies governance files (systems/constraints/registry/constraints/rules system files).

Rule:
- MUST run `node scripts/lint-governance.mjs` before committing or pushing when any of these paths changed:
  - `systems/constraints/registry/`
  - `systems/constraints/constraints/`
  - `systems/constraints/rules/`
  - `systems/constraints/CONSTRAINTS.md`
  - `systems/constraints/CONSTRAINTS_SYSTEM.md`
  - `scripts/lint-governance.mjs`
- SHOULD install local git hooks to enforce the gate automatically:
  - `bash scripts/install-git-hooks.sh`
- MUST report the lint result when committing/pushing:
  - "Governance lint: OK" OR
  - "Governance lint: FAIL" + the first relevant error lines

Rationale:
- Governance files define how the agent routes and applies rules. A broken link (registry -> constraints -> rules) creates silent failures.
- A lightweight local gate reduces drift without requiring GitHub Actions or CI.

Examples:
- Before commit:
  - `node scripts/lint-governance.mjs`
  - If OK: proceed to `git commit ...`
- With hooks installed:
  - The pre-commit/pre-push hooks run the lint automatically.

Failure Modes:
- Lint fails -> do not commit/push; fix the referenced path or pack mapping, re-run lint.
- Hooks not installed -> install once with `bash scripts/install-git-hooks.sh`.

Related:
- `systems/constraints/rules/repo-hygiene/00-INDEX.md`
- [REPO-010] Write and Commit Authorization
- `scripts/lint-governance.mjs`
- `scripts/install-git-hooks.sh`
