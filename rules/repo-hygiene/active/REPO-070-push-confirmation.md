# systems/constraints/rules/repo-hygiene/active/REPO-070-push-confirmation.md

Title: Push Requires Explicit Confirmation

ID: REPO-070
Status: active
Pack: repo-hygiene
Owner: user
Last Updated: 2026-03-05

When:
- In MAIN SESSION, any time the agent is preparing to run `git push`.

Rule:
- The agent MUST ask for explicit confirmation before each `git push`.
- Generic acknowledgements (e.g., "start", "continue", "agree") do not count as push authorization.
- The user MAY grant a scoped auto-push authorization, but the scope MUST be explicit (e.g., number of commits or a clear stop condition).
- In SHARED CONTEXT, the agent MUST NOT push.

Rationale:
- Even when the remote is private and direct push is generally allowed, the user should have a last-mile decision gate for outbound changes.

Related:
- [REPO-010] Write and Commit Authorization
- [REPO-050] Post-Commit Push Prompt
