# systems/constraints/constraints/docker-control/00-INDEX.md

Pack:
- docker-control

Purpose:
- Provide routing + compliance checks for Docker control actions.
- Cover Docker operations performed through MCP servers, Docker SDK clients, Docker CLI, or any other path that can create, modify, remove, or expose containerized workloads.

Triggers (examples):
- User asks to deploy, start, stop, recreate, or remove containers.
- User asks to pull/build images, manage networks/volumes, or run Compose-like workflows.
- Agent is about to use `mcp-server-docker`, Docker CLI, or another tool that controls a Docker daemon.
- Agent is about to control a remote Docker engine on another machine.

Minimal reads:
- MUST load the docker-control rules pack entrypoint: `systems/constraints/rules/docker-control/00-INDEX.md`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation (reading systems/constraints/rules/constraints outside this pack) requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Connection target / remote engine method / identity checks: `systems/constraints/rules/docker-control/active/DOCKER-010-connection-and-target-verification.md`
- Image provenance / tag pinning / trust checks: `systems/constraints/rules/docker-control/active/DOCKER-020-image-trust-and-versioning.md`
- Volume mounts / host path restrictions: `systems/constraints/rules/docker-control/active/DOCKER-030-volume-and-host-path-restrictions.md`
- Port exposure / network blast radius: `systems/constraints/rules/docker-control/active/DOCKER-040-network-and-port-exposure.md`
- Privileged mode / dangerous capabilities / socket mounts: `systems/constraints/rules/docker-control/active/DOCKER-050-dangerous-capabilities.md`
- Lifecycle / labels / cleanup / drift control: `systems/constraints/rules/docker-control/active/DOCKER-060-lifecycle-and-cleanup.md`
- Receipts / approvals / verification requirements: `systems/constraints/rules/docker-control/active/DOCKER-070-receipts-and-verification.md`

Checks (reference-first):
- Verify applicable constraints in `systems/constraints/rules/docker-control/active/DOCKER-010-connection-and-target-verification.md` before touching any Docker daemon.
- MUST treat Docker control as high-risk because Docker access can affect the host system, persistent storage, local network exposure, and service availability.
- MUST require explicit user intent for any create/update/delete/recreate/publish action.
- MUST require a second explicit confirmation before destructive lifecycle actions that can stop, replace, or remove an existing workload, including `remove`, `recreate`, `destroy`, destructive image deletion, network deletion, and volume deletion.
- MUST avoid exposing container ports broadly or mounting sensitive host paths unless explicitly requested and confirmed.
- MUST treat Docker socket mounts (for example `/var/run/docker.sock`) as ultra-high-risk host-control escalation and prohibit them by default unless the user explicitly requests that exact mount and separately confirms it.
- MUST prefer the least-exposed connection method to a remote Docker engine (for example `ssh://...` over an unauthenticated TCP daemon).
- Reporting receipt (include in the user-visible response for high-risk Docker actions):
  - Applied rules: DOCKER-010[, DOCKER-020][, DOCKER-030][, DOCKER-040][, DOCKER-050][, DOCKER-060][, DOCKER-070]
  - Checks performed: target verified (DOCKER-010); image trust/tag review (DOCKER-020) when applicable; volume/path safety check (DOCKER-030) when applicable; port/network exposure review (DOCKER-040) when applicable; dangerous capability check (DOCKER-050) when applicable; lifecycle/labels/cleanup plan (DOCKER-060); receipt/verification completed (DOCKER-070)

Authoritative references:
- `systems/constraints/rules/docker-control/00-INDEX.md`
- `systems/constraints/registry/docker.yml`
