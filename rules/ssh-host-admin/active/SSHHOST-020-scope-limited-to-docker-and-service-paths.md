Title: Scope Limited to Docker and Service Paths

ID: SSHHOST-020
Status: active
Pack: ssh-host-admin
Owner: user
Last Updated: 2026-03-08

When:
- Before or during host-terminal SSH work.

Rule:
- MUST limit host-terminal SSH work to Docker resources and directly related service paths for the currently authorized service/task.
- MAY inspect and modify:
  - Docker containers, images, volumes, networks, and logs relevant to the authorized task
  - service configuration files directly tied to the authorized Docker-managed service
  - files inside explicitly identified Docker volume mountpoints for the authorized service
- MUST NOT inspect or modify unrelated host directories, user files, or system configuration.
- MUST stop and ask if the task would require leaving Docker / target-service scope.

Rationale:
- Host-terminal SSH is too powerful to leave scope implicit.
- Narrowing the allowed surface area keeps the host boundary understandable and auditable.

Examples:
- Allowed:
  - Inspect `docker ps`, `docker logs searxng`, `docker volume inspect searxng-data`
  - Edit `/var/lib/docker/volumes/searxng-data/_data/settings.yml` for the authorized `searxng` service
- Not allowed:
  - Browsing unrelated files under the user's home directory
  - Editing unrelated host services or generic system config

Failure Modes:
- The agent starts in Docker scope but drifts into unrelated host paths -> violation.
- The agent reads broadly “just to look around” on the host -> violation.

Related:
- `systems/constraints/rules/ssh-host-admin/00-INDEX.md`
- [SSHHOST-030] No Destructive Host Operations
