# systems/constraints/constraints/web-search/00-INDEX.md

Pack:
- web-search

Purpose:
- Provide routing + compliance checks for web search and external URL reading.

Triggers (examples):
- User asks to search: "搜索/检索/查资料/找链接/用互联网搜"
- Agent needs external sources to answer accurately.

Minimal reads:
- MUST load the web-search rules pack entrypoint: `systems/constraints/rules/web-search/00-INDEX.md`.
- MUST load the web-search runtime settings: `systems/constraints/constraints/web-search/settings.json`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation (reading systems/constraints/rules/constraints outside this pack) requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Backend selection, fallback conditions, and URL read caps: `systems/constraints/rules/web-search/active/WEB-010-web-search-strategy.md`
- Query variant strategy: `systems/constraints/rules/web-search/active/WEB-011-query-variant-strategy.md`
- Authoritative source routing: `systems/constraints/rules/web-search/active/WEB-012-authoritative-source-routing.md`
- Result quality gate: `systems/constraints/rules/web-search/active/WEB-013-search-result-quality-gate.md`

Checks (reference-first):
- Verify applicable constraints in `systems/constraints/rules/web-search/active/WEB-010-web-search-strategy.md` before searching/reading.
- Verify applicable constraints in `systems/constraints/rules/web-search/active/WEB-011-query-variant-strategy.md` when generating query variants.
- Verify applicable constraints in `systems/constraints/rules/web-search/active/WEB-012-authoritative-source-routing.md` when narrowing toward a known source.
- Verify applicable constraints in `systems/constraints/rules/web-search/active/WEB-013-search-result-quality-gate.md` before expanding to follow-up reads or fallback.
- Apply runtime-tunable strategy parameters from `systems/constraints/constraints/web-search/settings.json` when they do not conflict with active rules.
- Reporting receipt (include in the user-visible response when web search materially affects the answer):
  - Applied rules: WEB-010[, WEB-011][, WEB-012][, WEB-013]
  - Checks performed: primary backend first; query variants bounded by strategy; authoritative-source routing when applicable; result-quality review before deeper reads; browser fallback reason if used

Authoritative references:
- `systems/constraints/rules/web-search/00-INDEX.md`
- `systems/constraints/registry/web-search.yml`
