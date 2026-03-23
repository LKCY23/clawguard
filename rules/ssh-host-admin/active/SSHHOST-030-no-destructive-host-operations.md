Title: No Destructive Host Operations

ID: SSHHOST-030
Status: active
Pack: ssh-host-admin
Owner: user
Last Updated: 2026-03-08

When:
- Before running any host-terminal SSH command that could delete, broadly modify, or disable resources.

Rule:
- MUST NOT perform destructive host operations by default.
- MUST NOT delete or broadly modify unrelated host files, directories, services, or Docker resources.
- MUST treat the following as destructive unless separately and explicitly authorized:
  - removing unrelated containers, images, volumes, or networks
  - Docker prune-style cleanup commands
  - file deletion outside the currently authorized service scope
  - disabling or changing unrelated host services
- Even with host-terminal SSH authorization, destructive operations remain separately gated.

Rationale:
- SSH into the host terminal creates a much larger blast radius than ordinary local work.
- The user may authorize inspection or narrow service repair without authorizing broad cleanup or host mutation.

Examples:
- Not allowed by default:
  - `docker system prune -a`
  - `rm -rf` on host paths
  - removing unrelated containers or volumes

Failure Modes:
- The agent conflates “SSH allowed” with “destructive host admin allowed” -> violation.
- The agent performs cleanup to be helpful without specific approval -> violation.

Related:
- `systems/constraints/rules/ssh-host-admin/00-INDEX.md`
- [SSHHOST-020] Scope Limited to Docker and Service Paths
- [SSHHOST-040] Host Terminal Requires Explicit Authorization
