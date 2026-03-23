# systems/constraints/rules/ssh-host-admin/00-INDEX.md

Purpose:
- Define how the agent may use SSH to access the host terminal.
- Keep host-terminal SSH usage explicitly authorized, tightly scoped, and auditable.

Non-goals / Out of scope:
- Ordinary local exec that does not enter the host terminal over SSH.
- Docker MCP usage by itself when no host shell is opened.
- General remote administration beyond the specifically authorized host target.

Entry Decision Tree (navigation-first):
1) Is the requested action actually SSH into the host terminal?
   - If no, do not use this pack.
2) Is the SSH target explicitly allowed?
   - If no, stop.
3) Has the user explicitly authorized host-terminal SSH for this action?
   - If no, stop and ask.
4) Is the intended work limited to Docker / target-service-related scope?
   - If no, stop and narrow scope.
5) Is there a safer non-SSH alternative available?
   - If yes, prefer the safer path.
6) Before acting, emit a preflight receipt.

Terms:
- host terminal: an interactive or non-interactive shell session reached on the host machine via SSH.
- allowed target: an SSH host alias or target explicitly approved by the user for this pack.
- target-service scope: work limited to Docker resources and directly related service files/paths for the currently authorized service.
- destructive host action: any action that can delete, broadly modify, or disable unrelated host resources.

Decision Order (within this pack):
1) Explicit authorization for host-terminal SSH use
2) Scope limitation to Docker / target-service-related work
3) No destructive or unrelated host changes
4) Auditability via preflight and exact command reporting

Active Rules (default-enforced):
- [SSHHOST-010] Host Target Allowlist — Only connect to explicitly approved host SSH targets
- [SSHHOST-020] Scope Limited to Docker and Service Paths — Restrict host-shell work to Docker and directly related service scope
- [SSHHOST-030] No Destructive Host Operations — Prohibit destructive or unrelated host changes by default
- [SSHHOST-040] Host Terminal Requires Explicit Authorization — Always ask before using the host terminal over SSH
- [SSHHOST-050] Preflight and Command Reporting — Emit a preflight receipt and exact command summary for host-shell actions
