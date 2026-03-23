Title: Persistent Actions Require Self-Check Receipt

ID: REPO-090
Status: active
Pack: repo-hygiene
Owner: user
Last Updated: 2026-03-08

When:
- The agent is about to make any persistent local change.
- The agent is about to edit configuration that affects later behavior.
- The agent is about to change daemon/service-related local state.
- The agent believes the change is “small”, “quick”, or “part of investigation”, but the change still leaves durable state.

Rule:
- In MAIN SESSION:
  - MUST provide a short self-check / receipt before executing any persistent local change.
  - MUST include at least:
    - intent
    - target file/path/system surface
    - why the change is needed now
    - how the result will be verified
  - MUST NOT treat “small change”, “quick fix”, “while investigating”, or “just config” as an exemption.
  - MAY proceed without waiting for further user approval when prior authorization for the category of action already exists.
- In SHARED CONTEXT:
  - MUST NOT perform persistent writes.

Rationale:
- High-impact mistakes often happen in “small” local changes that feel too minor to narrate.
- A short pre-action self-check improves auditability, success rate, and recovery without forcing unnecessary approval loops.

Examples:
- Before editing a config:
  - `Intent: enable host-docker keep-alive for 15 minutes; Target: ~/.mcporter/mcporter.json; Verify: mcporter config doctor`
- Before changing local daemon-managed behavior:
  - `Intent: enable daemon-managed keep-alive reuse; Target: local mcporter config; Verify: daemon status + repeated call timing`

Failure Modes:
- The agent silently edits a persistent config during debugging -> violation, even if the change is correct.
- The agent only explains the change afterward -> insufficient unless clearly marked as a backfilled receipt after an accidental miss.

Related:
- [REPO-010] Write and Commit Authorization
- `systems/constraints/CONSTRAINTS_RUNTIME.md`
- `systems/constraints/rules/repo-hygiene/00-INDEX.md`
