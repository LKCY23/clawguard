Title: Connection and Target Verification

ID: DOCKER-010
Status: active
Pack: docker-control
Owner: user
Last Updated: 2026-03-07

When:
- Before connecting to any Docker daemon.
- Before running any Docker action against a local or remote engine.

Rule:
- MUST identify the exact Docker target before taking action.
- MUST distinguish between a local daemon and a remote engine.
- MUST prefer the least-exposed connection method for remote control.
- MUST prefer `ssh://...` over raw TCP daemon access unless the user explicitly requests another authenticated method.
- MUST NOT use an unauthenticated or ambiguous Docker endpoint.
- MUST NOT proceed if the target host, daemon, or connection method is unclear.
- MUST state the target Docker host in receipts for non-trivial actions.
- SHOULD verify that the intended daemon is reachable before attempting mutating actions.
- SHOULD avoid reusing stale assumptions about the current Docker target across unrelated tasks.

Rationale:
- Docker control is only safe when the operator knows exactly which daemon is being modified.
- Remote Docker mistakes can impact the wrong machine, the wrong workload, or the wrong environment.
- Safer connection methods reduce exposure and configuration drift.

Examples:
- Safer remote target:
  - `DOCKER_HOST=ssh://user@host`
- Unsafe target:
  - an unauthenticated raw TCP Docker daemon exposed broadly on the network
- Receipt wording:
  - `Target Docker host: ssh://user@host`

Failure Modes:
- The target is implied but never verified -> stop and identify the exact daemon first.
- The agent assumes the Docker host from prior context -> restate and verify before mutation.
- A raw TCP endpoint is offered without authentication/TLS -> prefer SSH or stop and escalate.

Related:
- `systems/constraints/rules/docker-control/00-INDEX.md`
- `systems/constraints/rules/docker-control/active/DOCKER-070-receipts-and-verification.md`
