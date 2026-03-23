Title: Preflight and Command Reporting

ID: SSHHOST-050
Status: active
Pack: ssh-host-admin
Owner: user
Last Updated: 2026-03-08

When:
- Before and after host-terminal SSH actions.

Rule:
- Before using host-terminal SSH, MUST provide a preflight self-check / receipt that includes at least:
  - target host
  - exact scope of work
  - command family or action type
  - expected state change (or explicit read-only statement)
  - verification plan
- After using host-terminal SSH, MUST report:
  - the exact command(s) used, or a concise but faithful summary
  - what changed (or that nothing changed)
  - what was verified
- SHOULD keep host-terminal SSH reporting concise but auditable.
- MUST make it easy for the user to reconstruct what happened on the host if something goes wrong later.

Rationale:
- Host-terminal SSH is a high-value boundary crossing.
- Preflight + exact reporting improves auditability and helps the user distinguish careful, scoped host work from opportunistic shell usage.

Examples:
- Preflight:
  - `Intent: read SearXNG config on host via ssh host-docker; Scope: Docker volume path for searxng only; Verify: read back target lines`
- Post-action:
  - `Used ssh host-docker to inspect docker volume mountpoint and read settings.yml lines 70-95; no host files outside service scope were modified`

Failure Modes:
- The agent says only “I used SSH” without exact scope or command detail -> insufficient.
- The agent reports only success/failure but not what host commands ran -> insufficient.

Related:
- `systems/constraints/CONSTRAINTS_RUNTIME.md`
- `systems/constraints/rules/ssh-host-admin/00-INDEX.md`
- [SSHHOST-040] Host Terminal Requires Explicit Authorization
