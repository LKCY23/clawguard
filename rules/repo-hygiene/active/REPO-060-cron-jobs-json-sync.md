# systems/constraints/rules/repo-hygiene/active/REPO-060-cron-jobs-json-sync.md

Title: Cron Jobs JSON Sync

ID: REPO-060
Status: active
Pack: repo-hygiene
Owner: user
Last Updated: 2026-03-05

When:
- In MAIN SESSION, any time the agent is preparing to create a git commit in this repo.

Rule:
- The agent MUST check whether `cron/jobs.json` has changed before committing.
- If `cron/jobs.json` has changed, the agent MUST include it in the same commit (unless the user explicitly says not to).
- If `cron/jobs.json` has changed but is not intended to be committed, the agent MUST call it out and ask the user how to proceed.
- In SHARED CONTEXT, the agent MUST NOT commit or push.

Rationale:
- `cron/jobs.json` is the durable cron configuration and can change implicitly when jobs run.
- Tracking it in git improves portability and makes behavior reproducible across machines.

Notes:
- This rule is about commits, not about pushing.
- If `cron/jobs.json` churn becomes noisy, consider a separate normalization workflow; do not silently drop changes.

Related:
- [REPO-010] Write and Commit Authorization
- [REPO-050] Post-Commit Push Prompt
