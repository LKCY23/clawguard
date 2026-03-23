Title: Remote Push Authorization

ID: REPO-040
Status: active
Pack: repo-hygiene
Owner: user
Last Updated: 2026-03-04

When:
- The agent is about to run `git push` (any remote).

Rule:
- MUST confirm the remote address and intended ref(s) (branch/tag) before pushing.
- MUST confirm whether the destination remote is private.
- MUST check whether the push includes files that contain secrets.
  - If the push would include secret-bearing files, MUST explicitly confirm with the user before pushing.
- MUST default to NOT pushing secret-bearing configuration files.
  - Exception: files explicitly intended to be tracked (e.g., documented as tracked in `.gitignore`/repo policy). Even then, if secrets are present or the user requests pushing secret-bearing config, MUST confirm with the user before pushing.

Rationale:
- Pushing is an external action and may be irreversible; secrets committed to a remote can be hard to fully revoke.

Examples:
- "About to push to origin (https://github.com/<owner>/<repo>) branch main. This includes openclaw.json which contains tokens. Confirm this is a private repo and you want it pushed."

Related:
- [REPO-010] Write and Commit Authorization
- [REPO-030] Governance Lint Gate
