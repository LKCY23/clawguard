# systems/constraints/rules/repo-hygiene/00-INDEX.md

Purpose:
- Define rules for working with git repositories and making persistent changes in the workspace.

Non-goals / Out of scope:
- How to use `functions.exec` in detail (see `systems/constraints/rules/exec-tool/`).
- Project-specific branching/release workflows.

Entry Decision Tree (navigation-first):
- Is this a SHARED context? If yes: no writes, no commits, suggestions only.
- Is this a MAIN session?
  - If only drafting: do not write to disk.
  - If writing persistent state: emit a short self-check / receipt before acting.
  - If writing to disk: do the minimal change.
  - If committing: only when explicitly asked.
  - If pushing: only when explicitly asked.

Terms:
- write: create/edit/move files in the workspace.
- commit: `git commit` creating a new revision.
- push: `git push` sending commits to a remote.
- MAIN session: direct/private chat with the user.
- SHARED context: group chats or shared surfaces.

Decision Order (within this pack):
1) Privacy and safety boundaries (SHARED is read-only)
2) User intent and explicit authorization for persistent actions
3) Minimal diffs and traceability

Active Rules (default-enforced):
- [REPO-010] Write and Commit Authorization — Do not commit/push unless explicitly asked
- [REPO-020] Projects Under `repositories/` MUST Be Git Submodules — Track `repositories/*` as submodules (parent stores only commit pointers)
- [REPO-030] Governance Lint Gate — Run governance lint before commit/push when governance files change
- [REPO-040] Remote Push Authorization — Confirm remote privacy and secrets before pushing
- [REPO-050] Post-Commit Push Prompt — After creating a commit, prompt whether to push (do not push by default)
- [REPO-060] Cron Jobs JSON Sync — Include `cron/jobs.json` changes in commits by default
- [REPO-070] Push Requires Explicit Confirmation — Always ask before each `git push`
- [REPO-080] OpenClaw Upgrade Drift First-Check — Verify current OpenClaw contracts before rewriting local integration logic after upgrades
- [REPO-090] Persistent Actions Require Self-Check Receipt — Persistent local changes need a pre-action self-check even without approval gating

Verification:
- Install local git hooks: `bash scripts/install-git-hooks.sh`
- Run governance lint: `node scripts/lint-governance.mjs`

Draft Rules (not default):
- (none)

Notes:
- If rules in this pack conflict, apply this pack’s Decision Order first; if conflict remains, apply the
  global conflict resolution order (see `systems/constraints/CONSTRAINTS_SYSTEM.md`).
