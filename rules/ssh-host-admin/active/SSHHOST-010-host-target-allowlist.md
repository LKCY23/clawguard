Title: Host Target Allowlist

ID: SSHHOST-010
Status: active
Pack: ssh-host-admin
Owner: user
Last Updated: 2026-03-08

When:
- Before the agent uses SSH to enter a host terminal.

Rule:
- MUST only connect to SSH targets explicitly approved by the user for host-terminal use.
- MUST NOT infer or expand the target set on its own.
- MUST restate the exact target in the preflight receipt.
- The currently allowed target for this pack MAY be limited to a single alias (for example `host-docker`) when the user wants a narrow control boundary.

Rationale:
- SSH target ambiguity creates unnecessary blast radius.
- A small allowlist keeps host-terminal access reviewable and predictable.

Examples:
- Allowed:
  - `ssh host-docker 'docker ps'` when `host-docker` is the explicitly approved target.
- Not allowed:
  - Switching to another SSH alias or raw host/IP without explicit approval.

Failure Modes:
- The agent assumes any working SSH target is acceptable -> violation.
- The agent broadens from one approved host alias to multiple targets -> violation.

Related:
- `systems/constraints/rules/ssh-host-admin/00-INDEX.md`
- [SSHHOST-040] Host Terminal Requires Explicit Authorization
