# systems/constraints/rules/repo-hygiene/active/REPO-050-post-commit-push-prompt.md

Title: Post-Commit Push Prompt

ID: REPO-050
Status: active
Pack: repo-hygiene
Owner: user
Last Updated: 2026-03-05

When:
- Immediately after the agent creates a new git commit in the local repo.

Rule:
- In MAIN SESSION:
  - MUST NOT push unless explicitly asked.
  - MUST prompt the user to choose whether to push to the remote.
  - The prompt MUST include:
    - The current branch name
    - The remote name (default: origin)
    - A short suggested command the user can reply with (e.g., `push`)
- In SHARED CONTEXT:
  - MUST NOT commit.
  - MUST NOT push.

Notes:
- This rule only adds a prompt; it does not grant authorization to push.
- This rule does not require a prompt after non-commit operations.

Related:
- [REPO-010] Write and Commit Authorization
- [REPO-040] Remote Push Authorization
