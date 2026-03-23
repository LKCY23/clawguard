# CONSTRAINTS.md

This file defines the global constraint model for the agent.

For system structure and reading order, see `CONSTRAINTS_OVERVIEW.md`.

Authoritative for:
- Defining the constraint model and its vocabulary (domain vs pack).
- Defining what constraint entrypoints are and what they are allowed to contain.
- Providing a high-level pack taxonomy (map), as a convenience.

Not authoritative for:
- The runtime procedure for applying constraints or producing receipts (see `systems/constraints/CONSTRAINTS_RUNTIME.md`).
- Governance, conflicts, and rule lifecycle (see `systems/constraints/CONSTRAINTS_SYSTEM.md`).
- Domain behavior rules (those live under `systems/constraints/rules/<pack>/...``).

Goal:
- Constraints are cross-cutting guardrails.
- They apply to actions regardless of whether a Flow exists.
- Flows SHOULD comply, but non-flow work MUST also comply.

Execution model (lightweight):
1) Identify the next pack you are about to apply (via registry routing).
2) Before performing the action, read the corresponding constraint entry under `systems/constraints/constraints/<pack>/00-INDEX.md`.
3) That constraint entry defines the minimal reads into `systems/constraints/rules/<pack>/*` and the compliance checks.
4) Perform the action while honoring MUST/SHOULD constraints.
5) If missing required context (routing, credentials, target), ask the user a clarifying question instead of guessing.

Domain vs Pack:
- domain: a registry routing category used to match user intent (keywords/triggers).
  - Routing truth lives in `systems/constraints/registry/*.yml`.
- pack: a named ruleset with an authoritative rules folder `systems/constraints/rules/<pack>/` and a constraints entrypoint `systems/constraints/constraints/<pack>/`.
  - Constraints entry truth lives in `systems/constraints/constraints/<pack>/00-INDEX.md`.
  - Rules truth lives in `systems/constraints/rules/<pack>/...``.

Pack taxonomy (current):
- Operational packs:
  - `exec-tool`
  - `repo-hygiene`
  - `web-search`
- Governance packs:
  - `rules-governance`
  - `artifacts`
  - `maintenance`
- Reserved (not yet implemented):
  - `messaging`
  - `doc`

Actions that require constraints (initial set):
- `web-search`        -> `systems/constraints/constraints/web-search/00-INDEX.md`
- `exec-tool`         -> `systems/constraints/constraints/exec-tool/00-INDEX.md`
- `repo-hygiene`      -> `systems/constraints/constraints/repo-hygiene/00-INDEX.md`
- `rules-governance`  -> `systems/constraints/constraints/rules-governance/00-INDEX.md`
- `artifacts`         -> `systems/constraints/constraints/artifacts/00-INDEX.md`
- `maintenance`       -> `systems/constraints/constraints/maintenance/00-INDEX.md`

Persistent config changes and daemon/service lifecycle changes MUST be treated as constrained state-changing actions even when local in scope.

Single source of truth:
- Routing: `systems/constraints/registry/*.yml`
- Constraint entrypoints: `systems/constraints/constraints/<pack>/00-INDEX.md`
- Domain rules: `systems/constraints/rules/<pack>/...``
- Runtime procedure + receipts: `systems/constraints/CONSTRAINTS_RUNTIME.md`
- Governance + conflicts + lifecycle: `systems/constraints/CONSTRAINTS_SYSTEM.md`

Pack-local settings:
- A constraint pack MAY define pack-local runtime settings files under `systems/constraints/constraints/<pack>/`.
- These settings files MAY be referenced by the pack entrypoint when execution requires tunable parameters.
- Such settings files are not a replacement for rules and MUST NOT define normative behavior on their own.
- The authoritative split remains:
  - rules define principles and obligations
  - settings define tunable parameters within those rule boundaries

Notes:
- Constraint docs MUST NOT duplicate rules pack content. They define triggers, routing/enforcement, and compliance checks, and link to the authoritative rules packs.
- Constraint docs MAY contain MUST/SHOULD/MAY statements ONLY for orchestration (e.g., required reads, escalation gates, or checklists).
  They MUST NOT introduce domain-level behavioral rules; those belong in `systems/constraints/rules/<pack>/*`.
- Receipt requirements and format are defined in `systems/constraints/CONSTRAINTS_RUNTIME.md`.
- Conflicts are resolved per `systems/constraints/CONSTRAINTS_SYSTEM.md`.
