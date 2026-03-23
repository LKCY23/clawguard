# Title

OpenClaw Upgrade Drift First-Check

## ID

REPO-080

## Status

active

## Pack

repo-hygiene

## Owner

user

## Last Updated

2026-03-06

## When

Applies when a workspace hook, repository-local integration, or local automation regresses after an OpenClaw upgrade and depends on OpenClaw internals, especially for workspace hooks and repository-local automations built on OpenClaw internals such as:
- hook event shape
- hook context fields
- CLI subcommands or behavior
- tool policy or approvals behavior

## Rule

- When an OpenClaw-based integration regresses after an OpenClaw upgrade, the operator MUST first suspect interface drift at the OpenClaw boundary before changing repository-local business logic.
- The operator MUST verify the current local contract using the installed version’s docs and local `dist/` implementation before modifying hook or repository logic.
- The operator SHOULD check:
  - event `type` / `action`
  - context field availability
  - CLI surface changes
  - tool policy / approvals behavior
- When the failure mode is unclear, the operator SHOULD add a minimal runtime probe before broader changes.
- The operator MUST NOT conclude that local business logic is wrong until interface drift has been ruled out.

## Rationale

OpenClaw upgrades can change internal hook contracts and runtime behavior without any change to repository-local code. Verifying the current OpenClaw contract first prevents misdiagnosis and unnecessary local rewrites.

## Examples

### Example 1

Observed:
- a bootstrap hook still registers and enters
- but the main logic no longer runs

Correct first move:
- verify the current event shape in local docs / `dist/`
- then update the local event guard only if needed

### Example 2

Observed:
- a message hook fires
- but expected fields like `workspaceDir` are missing

Correct first move:
- verify the current event/context contract
- then choose the correct event stage before changing local logic

## Failure Modes

- misdiagnosing interface drift as local logic failure
- rewriting repository-local code before checking the current OpenClaw contract
- relying on stale assumptions from older OpenClaw versions

## Related

- `systems/constraints/rules/repo-hygiene/00-INDEX.md`
- `systems/constraints/CONSTRAINTS_RUNTIME.md`
- `CONSTRAINTS_OVERVIEW.md`
