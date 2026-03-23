Title: Volume and Host Path Restrictions

ID: DOCKER-030
Status: active
Pack: docker-control
Owner: user
Last Updated: 2026-03-07

When:
- Before creating or recreating any container that uses volumes or bind mounts.
- Before creating or removing persistent Docker volumes.

Rule:
- MUST distinguish between named Docker volumes and host path bind mounts.
- MUST prefer named Docker volumes over host path bind mounts when feasible.
- MUST treat host path bind mounts as higher-risk than named volumes.
- MUST NOT mount sensitive host paths by default.
- Sensitive host paths include at minimum:
  - `/`
  - user home directories
  - SSH key directories
  - OpenClaw config or workspace directories
  - Docker daemon sockets
  - other credential-bearing or system-critical paths
- MUST require explicit user intent before using any host path bind mount.
- MUST require additional scrutiny before reusing an existing host path with unknown contents.
- SHOULD describe the persistence model in the receipt when storage is relevant.
- SHOULD prefer creating a dedicated named volume for service data when possible.

Rationale:
- Host path mounts can expose sensitive files, couple containers tightly to host layout, and complicate cleanup.
- Named volumes are usually easier to reason about, safer to reuse, and clearer to audit.
- Storage choices affect both data safety and blast radius.

Examples:
- Safer:
  - a named volume for SearXNG data
- Higher-risk:
  - bind mounting a broad host directory into the container
- Disallowed by default:
  - mounting `~/.ssh` or `/var/run/docker.sock`

Failure Modes:
- The agent treats a host path like an ordinary volume -> escalate and confirm explicitly.
- The mount path is broad or sensitive -> stop and propose a named volume instead.
- The user asks for a quick bind mount -> explain the data exposure risk before proceeding.

Related:
- `systems/constraints/rules/docker-control/00-INDEX.md`
- `systems/constraints/rules/docker-control/active/DOCKER-050-dangerous-capabilities.md`
- `systems/constraints/rules/docker-control/active/DOCKER-060-lifecycle-and-cleanup.md`
