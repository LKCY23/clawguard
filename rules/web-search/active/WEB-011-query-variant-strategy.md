Title: Query Variant Strategy

ID: WEB-011
Status: active
Pack: web-search
Owner: user
Last Updated: 2026-03-09

When:
- The user asks to search the web, find sources, investigate an external issue, look up documentation, compare references, or gather links.
- The agent is about to issue a search query and the original query may not fully cover the likely source ecosystem.

Rule:
- MUST preserve the user's core intent.
- MUST NOT replace the user's original query by default when the original phrasing may itself be valuable.
- SHOULD treat the original query as a first-class search candidate.
- SHOULD generate query variants only when they are likely to improve coverage, precision, or source diversity.
- SHOULD use query variants as supplements to the original query, not as silent intent substitution.
- SHOULD allow variants across language, phrasing, target terms, and exact-error expressions when helpful.
- SHOULD prefer exact error text variants for error-driven searches.
- SHOULD keep query variants minimal, purposeful, and bounded.
- SHOULD avoid repeated rewrite loops when earlier variants do not materially improve result quality.
- MAY skip variant generation when the original query is already precise, versioned, or exact enough.

Rationale:
- Search quality depends strongly on query construction.
- Original-language queries can surface local or community-specific sources that translated queries may miss.
- Supplementary variants can improve recall and precision without erasing the user's original framing.

Examples:
- Original query retained plus an English variant for a software ecosystem whose official materials are mostly English.
- Original Chinese query retained because the user may want Chinese community results, while an English variant supplements official-source discovery.
- Exact error text kept as a dedicated variant instead of being paraphrased away.

Failure Modes:
- The agent silently replaces the user's query with a different framing and loses relevant local/community results -> retain the original query and compare quality.
- The agent keeps generating variants without improving quality -> stop and switch to quality review or source routing.
- The agent broadens the query until it drifts away from the user's target -> revert to a tighter variant set.

Related:
- `systems/constraints/rules/web-search/00-INDEX.md`
- `systems/constraints/rules/web-search/active/WEB-010-web-search-strategy.md`
- `systems/constraints/constraints/web-search/settings.json`
