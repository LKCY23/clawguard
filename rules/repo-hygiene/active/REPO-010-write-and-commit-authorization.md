# systems/constraints/rules/repo-hygiene/active/REPO-010-write-and-commit-authorization.md

Title: Write and Commit Authorization

ID: REPO-010
Status: active
Pack: repo-hygiene
Owner: user
Last Updated: 2026-02-25

When:
- Any time the agent considers making persistent changes (writing files, committing, pushing).

Rule:
- In SHARED CONTEXT / group chats:
  - MUST NOT create/edit/move files.
  - MUST NOT commit.
  - MUST NOT push.
  - SHOULD provide suggestions, drafts, or patch summaries only.
- In MAIN SESSION:
  - MUST NOT commit unless explicitly asked by the user.
  - MUST NOT push unless explicitly asked by the user.
  - SHOULD default to drafting changes in-chat first when the user is still reviewing structure/content.
  - MAY write files after the user requests or approves writing.

Rationale:
- Commits and pushes are durable actions that can change history, trigger automation, or leak information.
  Requiring explicit user authorization prevents accidental "speed-ups" that bypass review gates.

Examples:
- Allowed (explicit request):
  - User: "Commit these changes" -> Agent runs `git commit ...`.
  - User: "Push to origin" -> Agent runs `git push ...`.
- Not allowed (no explicit request):
  - Agent MUST NOT run `git commit` just because files were written.

Failure Modes:
- Agent accidentally committed -> immediately disclose it and offer to undo it (soft reset) while preserving working tree changes.

Related:
- `systems/constraints/rules/repo-hygiene/00-INDEX.md`
- `AGENTS.md` (SHARED write prohibition)
