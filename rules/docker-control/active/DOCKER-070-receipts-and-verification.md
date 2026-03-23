Title: Receipts and Verification

ID: DOCKER-070
Status: active
Pack: docker-control
Owner: user
Last Updated: 2026-03-07

When:
- The agent is about to create, update, recreate, stop, remove, or expose a Docker-managed workload.
- The agent changes Docker images, networks, volumes, or runtime settings in a way that affects availability, persistence, or host risk.

Rule:
- MUST produce a receipt for non-trivial Docker actions.
- MUST clearly identify the Docker target in the receipt.
- MUST include the affected resources in the receipt when known.
- MUST verify the resulting state after each non-trivial Docker action.
- MUST require explicit user intent before any create, update, recreate, stop, remove, or destroy action.
- MUST require a second explicit confirmation before destructive lifecycle actions that can stop, replace, remove, or materially disrupt an existing workload.
- The second explicit confirmation requirement applies at minimum to:
  - `remove_container`
  - `recreate_container`
  - destructive image removal
  - network removal
  - volume removal
  - any action described as `destroy`
  - any action that will replace a running production-like service
- MUST NOT treat prior broad approval as sufficient for a later destructive action unless the user clearly reaffirmed that exact destructive intent.
- MUST verify at least the following after applicable changes:
  - resource exists or was removed as intended
  - expected container status
  - expected image/tag
  - expected ports or exposure scope
  - expected labels
  - recent logs or health indicators when relevant
- SHOULD mention rollback or recovery posture when risk is non-trivial.

Rationale:
- Docker changes can alter host behavior, expose services, destroy state, or interrupt running workloads.
- Explicit receipts and verification reduce ambiguity and make high-risk automation auditable.
- A second confirmation helps prevent accidental deletion, recreation, or service interruption.

Examples:
- Safe create receipt:
  - Intent: Deploy host-side SearXNG container
  - Target Docker host: `ssh://user@host`
  - Applied: `DOCKER-010, DOCKER-020, DOCKER-040, DOCKER-060, DOCKER-070`
  - Verify: container running; expected port bound; expected labels present; logs show healthy startup
- Destructive action confirmation:
  - First user request: "recreate the SearXNG container with a new image"
  - Required second confirmation before execution: a clear follow-up confirmation that the running service may be replaced

Failure Modes:
- The agent proceeds from a vague request directly to deletion or recreation -> this is a policy failure; stop and obtain a second explicit confirmation.
- Verification is skipped after a risky change -> treat the operation as incomplete until verified.
- The resulting state differs from the plan -> report the deviation and do not silently continue.

Related:
- `systems/constraints/rules/docker-control/00-INDEX.md`
- `systems/constraints/rules/docker-control/active/DOCKER-010-connection-and-target-verification.md`
- `systems/constraints/rules/docker-control/active/DOCKER-060-lifecycle-and-cleanup.md`
