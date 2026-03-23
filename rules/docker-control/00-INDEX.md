# systems/constraints/rules/docker-control/00-INDEX.md

Purpose:
- Define how the agent controls Docker daemons, containers, images, networks, and volumes.
- Define how the agent manages remote Docker engines used to deploy or operate host-side services.

Non-goals / Out of scope:
- General shell execution safety (see `systems/constraints/rules/exec-tool/`).
- Generic git/workspace persistence (see `systems/constraints/rules/repo-hygiene/`).
- Non-container orchestration systems such as Kubernetes (unless a future pack explicitly covers them).
- Application-specific deployment semantics beyond Docker control itself.

Entry Decision Tree (navigation-first):
1) Identify the Docker target:
   - Local daemon on the current machine
   - Remote daemon on another machine
2) Verify the connection method:
   - Prefer least-exposed remote transport (`ssh://...` over raw TCP)
3) Determine intended operation:
   - Observe/list only
   - Create/update/recreate
   - Stop/remove/destroy
   - Expose ports / mount host paths / alter networks
4) Apply the smallest relevant safety rules:
   - Connection/target -> image trust -> volumes/paths -> ports/networks -> dangerous capabilities -> lifecycle/cleanup
5) Produce a receipt for high-risk actions.

Terms:
- Docker target: the specific Docker daemon being controlled.
- remote engine: a Docker daemon running on another machine, reached via SSH, TCP, TLS, context, or equivalent transport.
- dangerous capability: any setting that significantly expands host impact, such as privileged containers, host networking, Docker socket mounts, broad host path mounts, or extra Linux capabilities.
- `docker.sock` mount: mounting the Docker daemon socket (commonly `/var/run/docker.sock`) into a container so that the container can issue Docker API calls against the host daemon. This is effectively host-level Docker control and is therefore treated as a high-risk escalation.
- host path mount: any bind mount from the daemon host filesystem into a container.
- broad exposure: publishing a port on all interfaces or otherwise making a service reachable beyond the smallest necessary scope.
- managed resource: any container/image/network/volume created or changed by the agent and labeled for later audit/cleanup.
- label convention: the default metadata labels applied to agent-managed Docker resources so they can be audited, filtered, and safely cleaned up later.

Decision Order (within this pack):
1) Target correctness and operator intent
2) Host safety and least privilege
3) Network exposure minimization
4) Storage/path safety
5) Reproducibility (trusted images, stable names, labels)
6) Maintainability (small changes, explicit cleanup)
7) Convenience and speed

Active Rules (default-enforced):
- [DOCKER-010] Connection and Target Verification — Verify the exact Docker daemon and prefer least-exposed remote access
- [DOCKER-020] Image Trust and Versioning — Prefer trusted images and stable tags/digests when risk matters
- [DOCKER-030] Volume and Host Path Restrictions — Restrict bind mounts and sensitive host paths
- [DOCKER-040] Network and Port Exposure — Minimize published ports and exposure scope
- [DOCKER-050] Dangerous Capabilities — Block privileged mode, socket mounts, host network, and similar high-blast-radius settings by default
- [DOCKER-060] Lifecycle and Cleanup — Label managed resources and keep a cleanup/rollback path
- [DOCKER-070] Receipts and Verification — Record what changed and verify that the deployed state matches intent

Quick Priority Notes:
- If the requested Docker action could alter host integrity, leak data, or widen exposure, prefer the safer option even if it is less convenient.
- For remote Docker targets, connection safety and target correctness take precedence over deployment speed.
- For long-running services, reproducibility and explicit cleanup matter more than one-shot convenience.
- Default label convention for managed resources SHOULD be explicit and stable, for example:
  - `openclaw.managed=true`
  - `openclaw.project=<project-name>`
  - `openclaw.owner=agent`
  - `openclaw.purpose=<service-purpose>`

Notes:
- Docker control is high-risk by default because it can affect the host, persistent data, network exposure, and service availability.
- A future skill MAY wrap these rules with more task-specific workflows (for example, host-side SearXNG deployment), but this pack remains the authoritative safety layer.
- If rules in this pack conflict, apply this pack’s Decision Order first; if conflict remains, apply the global conflict resolution order (see `systems/constraints/CONSTRAINTS_SYSTEM.md`).
