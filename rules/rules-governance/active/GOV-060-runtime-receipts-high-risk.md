Title: Runtime Receipts for High-Risk Packs

ID: GOV-060
Status: active
Pack: rules-governance
Owner: user
Last Updated: 2026-03-04

When:
- Any time the agent performs a high-risk action governed by a pack constraints entry.

Rule:
- High-risk packs include:
  - `exec-tool`
  - `repo-hygiene`
  - `rules-governance`
  - `web-search` (only when web search materially affects the answer)
  - `artifacts` (only when artifacts are created/retained)
  - `maintenance` (only when maintenance actions run)
  - `messaging`
  - Reserved (when implemented): `doc`

- For high-risk actions, the agent MUST include an `Applied rules:` line listing the relevant rule IDs it followed.
- For high-risk actions, the agent SHOULD include a short `Checks performed:` line:
  - reference rule IDs
  - do not restate rule text
  - keep it to 1-2 short phrases

Rationale:
- Receipts create an auditable trail that the pack constraints were actually applied at runtime.

Examples:
- Exec:
  - Applied rules: EXEC-060, EXEC-050
  - Checks performed: destructive gate (EXEC-060); PTY/background strategy (EXEC-050)

Failure Modes:
- Missing receipts -> user cannot verify compliance; add Applied rules and the minimal checks performed.

Related:
- `systems/constraints/CONSTRAINTS.md`
- `systems/constraints/constraints/<pack>/00-INDEX.md`
