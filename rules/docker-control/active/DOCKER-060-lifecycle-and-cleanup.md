Title: Lifecycle and Cleanup

ID: DOCKER-060
Status: active
Pack: docker-control
Owner: user
Last Updated: 2026-03-07

When:
- The agent creates, recreates, updates, or removes Docker containers, networks, volumes, or related resources.
- The agent manages any long-running or stateful Docker workload.

Rule:
- MUST apply stable labels to every agent-managed Docker resource so the resource can be audited, filtered, and cleaned up later.
- MUST treat labels as part of the managed state, not as optional decoration.
- MUST use a default label convention unless the user explicitly asks for a different one.
- The default label convention SHOULD include at least:
  - `openclaw.managed=true`
  - `openclaw.project=<project-name>`
  - `openclaw.owner=agent`
  - `openclaw.purpose=<service-purpose>`
- MUST keep naming stable and predictable across retries and updates.
- MUST prefer update-in-place when safe and supported.
- MUST prefer explicit recreate only when an in-place update is not possible or would leave the workload inconsistent.
- MUST maintain a cleanup path for resources created by the agent.
- MUST avoid creating unlabeled orphan containers, unlabeled networks, or unlabeled volumes.
- SHOULD record which resources belong to the same project or service boundary through labels and stable names.
- SHOULD remove obsolete managed resources when they are no longer needed and when removal is explicitly intended and confirmed.

Rationale:
- Docker resources accumulate quickly and become hard to reason about without consistent labels and naming.
- Stable labels make it possible to audit changes, group related resources, and perform safe cleanup later.
- Lifecycle discipline reduces drift, orphaned resources, and accidental service breakage.

Examples:
- Managed SearXNG container labels:
  - `openclaw.managed=true`
  - `openclaw.project=searxng`
  - `openclaw.owner=agent`
  - `openclaw.purpose=search-backend`
- Predictable resource naming:
  - `searxng-app`
  - `searxng-data`
  - `searxng-net`
- Safe recreate:
  - Recreate a container only after verifying the replacement image, ports, volumes, and labels match the intended state.

Failure Modes:
- Resources are created without labels -> stop and correct the plan before creating more resources.
- Project resources drift apart over time -> re-group under stable labels and names before further mutation.
- Cleanup becomes unsafe because ownership is unclear -> do not guess; inspect labels and ask for confirmation if boundaries are uncertain.

Related:
- `systems/constraints/rules/docker-control/00-INDEX.md`
- `systems/constraints/rules/docker-control/active/DOCKER-030-volume-and-host-path-restrictions.md`
- `systems/constraints/rules/docker-control/active/DOCKER-070-receipts-and-verification.md`
