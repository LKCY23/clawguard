Title: Search Result Quality Gate

ID: WEB-013
Status: active
Pack: web-search
Owner: user
Last Updated: 2026-03-09

When:
- The agent has completed an initial search and must decide whether to read URLs, generate query variants, reroute toward a source, or fall back.

Rule:
- MUST evaluate result quality before treating a search as successful.
- MUST NOT treat non-empty results as sufficient evidence that the search is good enough.
- SHOULD perform a quick quality review over the leading result window before expanding work.
- SHOULD consider whether the results match the expected source type, stay on target, avoid obvious noise, and justify further reading cost.
- SHOULD treat repeated low-value domains, aggregator pages, reposts, thin summaries, and off-target results as negative quality signals.
- SHOULD prefer reading a small set of high-confidence URLs over mechanically reading results by position.
- SHOULD allow earlier transition to query variants, source rerouting, or fallback when the leading results are low-quality.
- SHOULD stop expanding when the authoritative or sufficient answer source is already clearly present.

Rationale:
- Practical search failure is often a quality failure rather than a zero-result failure.
- A result-quality gate reduces wasted reads, reduces noisy summaries, and improves when the agent chooses to reroute or fall back.

Examples:
- Results exist, but the leading pages are reposts and generic explainers -> do not treat the search as successful yet.
- Results immediately contain the official documentation page and the relevant tracker entry -> read those first and stop broadening the search.
- Results are present but clearly in the wrong language/community/source tier for the user's goal -> reroute or add variants.

Failure Modes:
- The agent reads many URLs because results were non-empty, even though the leading set was weak -> apply the quality gate earlier.
- The agent keeps broadening search despite already having a sufficient authoritative answer -> stop and answer.
- The agent misses high-quality local/community sources because it only rewards official sources -> evaluate fitness for the user's actual goal, not just institutional authority.

Related:
- `systems/constraints/rules/web-search/00-INDEX.md`
- `systems/constraints/rules/web-search/active/WEB-010-web-search-strategy.md`
- `systems/constraints/rules/web-search/active/WEB-011-query-variant-strategy.md`
- `systems/constraints/rules/web-search/active/WEB-012-authoritative-source-routing.md`
- `systems/constraints/constraints/web-search/settings.json`
