Title: Auto-Register Registry Entries When New Rule Packs/Roles Are Added

ID: GOV-020
Status: active
Pack: rules-governance
Owner: user
Last Updated: 2026-03-02

When:
- The agent creates a new rule pack under `systems/constraints/rules/<pack>/`.
- The agent activates a new rule (moves it to `systems/constraints/rules/<pack>/active/`).
- The agent modifies a pack index (`systems/constraints/rules/<pack>/00-INDEX.md`) in a way that adds new active rules.

Rule:
- MUST ensure rule navigation remains discoverable via a link in `systems/constraints/rules/README.md` when a new pack is created.

- MUST create a `systems/constraints/registry/<pack>.yml` entry whenever a pack has at least one Active Rule, unless a registry entry already covers that pack.

- MUST update `systems/constraints/registry/00-INDEX.md` to list the new entry when one is created.

- MUST treat newly created packs as allowlisted for auto-registration by default (no user reminder required).

- MUST NOT skip registration on the basis of "this pack might be one-off" if it contains Active Rules.

- SHOULD propose a conservative trigger list (5-15 triggers) in both English and Chinese.

- SHOULD set `minimalReadMain` and `minimalReadShared` to the pack index plus only the required active rules (least read).

- SHOULD set `outputPolicyShared.mode` to `summary-only`.

- MAY defer creating a registry entry only if the pack has no Active Rules (draft-only pack).

Rationale:
- Users should not need to remember to “also register” a new rule pack.
- Registry entries keep on-demand reads cheap and deterministic.

Examples:
- New pack `systems/constraints/rules/artifacts/` with active rules: create `systems/constraints/registry/artifacts.yml` and add it to `systems/constraints/registry/00-INDEX.md`.
- Adding a one-off rule to an existing pack that already has a registry entry: update the existing entry’s minimal reads if needed, but avoid expanding reads unnecessarily.

Failure Modes:
- Rule activated but not registered -> later the agent misses context; fix by adding `systems/constraints/registry/<pack>.yml` and listing it in the index.
- Registry bloat -> apply the MUST NOT clause for informational packs; keep triggers small.

Related:
- `AGENTS.md` (On-Demand Files Registry)
- `systems/constraints/registry/00-INDEX.md`
- `systems/constraints/CONSTRAINTS_SYSTEM.md`
