# systems/constraints/rules/web-search/00-INDEX.md

Purpose:
- Define how the agent performs web search and follow-up page reading.

Non-goals / Out of scope:
- General command execution safety (see `systems/constraints/rules/exec-tool/`).
- Git/workspace persistence (see `systems/constraints/rules/repo-hygiene/`).

Entry Decision Tree (navigation-first):
1) Choose the search backend:
   - Primary: MCP SearXNG via `mcporter` (`searxng.searxng_web_search`, `searxng.web_url_read`).
   - Fallback: openclaw-managed browser + Google search.
2) If results are empty:
   - Try one query rewrite (remove modifiers, switch language, add org name).
   - If still empty: fallback.
3) If you need content beyond snippets:
   - Read 1-5 URLs using `searxng.web_url_read`.
4) Report:
   - Provide sources (URLs) for claims.

Terms:
- MCP SearXNG: the MCP server configured in mcporter as `searxng`.
- primary search: default preferred path.
- fallback search: heavier, browser-driven path.

Decision Order (within this pack):
1) Use the primary backend unless a fallback condition is met.
2) Minimize reads (cap URL reads).
3) Cite sources.

Active Rules (default-enforced):
- [WEB-010] Web Search Strategy — Primary MCP SearXNG, fallback to managed browser search.
- [WEB-011] Query Variant Strategy — Preserve the original query while using bounded variants to improve coverage.
- [WEB-012] Authoritative Source Routing — Prefer known authoritative sources when the user's target is source-specific.
- [WEB-013] Search Result Quality Gate — Judge leading results by quality, not just non-empty output.

Draft Rules (not default):
- (none)

Notes:
- Conflicts: apply this pack's Decision Order first; if conflict remains, apply `systems/constraints/CONSTRAINTS_SYSTEM.md`.
