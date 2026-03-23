Title: Gitignore Tracking Hygiene

ID: GOV-080
Status: active
Pack: rules-governance
Owner: user
Last Updated: 2026-03-04

When:
- You change `.gitignore` (root or workspace `.gitignore`).

Rule:
- MUST review for files that were previously tracked but are now ignored, and remove them from the index if they should no longer be versioned.
  - Use: `git ls-files -ci --exclude-standard` to list tracked files that match ignore rules.
  - Remove from tracking with: `git rm --cached <path>` (do not delete the working copy).
- MUST NOT rely on ignore rules alone to stop tracking a file that is already tracked.

Rationale:
- Git ignore rules do not affect already-tracked files; without an explicit untrack step, migrations can leave stale artifacts in the repo.

Examples:
- After ignoring `cron/jobs.json.bak`, run `git ls-files -ci --exclude-standard` and then `git rm --cached cron/jobs.json.bak`.

Related:
- [GOV-050] Governance Lint Gate
