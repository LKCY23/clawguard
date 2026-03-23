Title: Host Terminal Requires Explicit Authorization

ID: SSHHOST-040
Status: active
Pack: ssh-host-admin
Owner: user
Last Updated: 2026-03-08

When:
- Any time the agent is about to use SSH to enter the host terminal.

Rule:
- MUST obtain explicit user authorization before any host-terminal SSH use.
- Generic acknowledgements such as “continue”, “go ahead”, or “okay” do NOT automatically authorize host-terminal SSH unless clearly scoped to that action.
- Authorization to use host-terminal SSH for one task does NOT grant standing permission for later unrelated tasks.
- The authorization check is stricter than ordinary local exec because host-terminal SSH crosses into a more powerful control boundary.

Rationale:
- Host-terminal SSH can reach beyond the current workspace and beyond Docker MCP boundaries.
- Requiring explicit authorization keeps the user in control of when that boundary is crossed.

Examples:
- Allowed:
  - User: “Use SSH to host-docker and fix the SearXNG config.”
- Not allowed:
  - Agent decides to SSH because Docker MCP editing feels inconvenient.

Failure Modes:
- The agent treats earlier Docker-related approval as automatic SSH approval -> violation.
- The agent uses SSH as a convenience fallback without explicit authorization -> violation.

Related:
- `systems/constraints/rules/ssh-host-admin/00-INDEX.md`
- [SSHHOST-010] Host Target Allowlist
- [SSHHOST-050] Preflight and Command Reporting
