Title: Dangerous Capabilities

ID: DOCKER-050
Status: active
Pack: docker-control
Owner: user
Last Updated: 2026-03-07

When:
- The agent is creating, running, recreating, or updating a container.
- The requested container configuration could significantly expand host impact or bypass normal isolation.

Rule:
- MUST treat the following as ultra-high-risk settings:
  - privileged containers
  - Docker socket mounts (for example `/var/run/docker.sock`)
  - host PID namespace
  - host network mode
  - broad capability additions
  - broad bind mounts of sensitive host paths
- MUST NOT use Docker socket mounts by default.
- MUST treat a Docker socket mount as more severe than ordinary high-risk configuration because it effectively grants the container control over the host Docker daemon.
- MUST NOT use a Docker socket mount unless the user explicitly requests it, understands the host-control implications, and provides a dedicated confirmation for that exact action.
- MUST strongly prefer alternative designs that avoid Docker socket mounts entirely.
- MUST NOT use `--privileged` by default.
- MUST NOT add broad Linux capabilities or use host namespaces by default.
- MUST NOT combine multiple ultra-high-risk settings in a single action unless the user explicitly requested and confirmed each relevant escalation.
- SHOULD refuse configurations that create unnecessary host takeover pathways when a safer design exists.
- SHOULD explain in one sentence why the setting is ultra-high-risk when blocking or escalating it.

Rationale:
- Some Docker settings do not merely widen exposure; they collapse isolation boundaries and create direct host-control pathways.
- Mounting `docker.sock` into a container effectively gives that container Docker control over the host, which can be chained into broad host impact.
- Ultra-high-risk settings require stronger friction than ordinary risky changes.

Examples:
- Block by default:
  - A request to mount `/var/run/docker.sock` into an application container so it can launch sibling containers
- Safer alternative:
  - Run a dedicated host-side control plane instead of passing host Docker control into an otherwise ordinary service container
- Escalation example:
  - If the user explicitly insists on a Docker socket mount, require a dedicated confirmation specific to that mount before proceeding

Failure Modes:
- The agent treats a Docker socket mount like an ordinary bind mount -> this is incorrect; it is an ultra-high-risk host-control escalation.
- The agent accepts `--privileged` because it seems convenient -> block and ask for a safer design or an explicit high-friction confirmation.
- The agent allows host namespace or broad capabilities without clear need -> stop and narrow the configuration.

Related:
- `systems/constraints/rules/docker-control/00-INDEX.md`
- `systems/constraints/rules/docker-control/active/DOCKER-030-volume-and-host-path-restrictions.md`
- `systems/constraints/rules/docker-control/active/DOCKER-070-receipts-and-verification.md`
