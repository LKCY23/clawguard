Title: Authoritative Source Routing

ID: WEB-012
Status: active
Pack: web-search
Owner: user
Last Updated: 2026-03-09

When:
- The user's request clearly targets a known authoritative source, such as official documentation, an official issue tracker, an official release page, a vendor knowledge base, or a product API reference.
- The agent can reasonably predict that narrowing to an authoritative source is more useful than broad open-web discovery.

Rule:
- SHOULD prefer routing toward a known authoritative source when the user's intent clearly targets that source type.
- SHOULD use source restriction as a precision tool rather than a universal default.
- SHOULD prefer domain-level source restriction over path-level restriction when narrowing to a source.
- MUST NOT assume that every search should be source-restricted.
- SHOULD allow open search when the user is exploring community experience, multilingual discussion, comparative research, or broad discovery.
- SHOULD relax or remove source restriction when it suppresses relevant results.
- MAY use common search-engine source restriction techniques, such as domain-level filtering, when they improve relevance.

Rationale:
- Known authoritative sources often provide higher-signal answers than broad web search.
- Over-restricting too early can hide valuable community discussions, multilingual guidance, and practical operator knowledge.
- Domain-level restriction is more robust than path-level restriction across search backends and indexing behavior.

Examples:
- For official product docs, route toward the official documentation domain first.
- For a bug or regression in a known project, route toward the project's issue tracker first.
- For community troubleshooting, avoid locking to a single official source too early.

Failure Modes:
- The agent over-constrains search and misses relevant community results -> widen or remove source restriction.
- The agent applies path-level restriction that yields brittle or empty results -> retry with domain-level restriction.
- The agent defaults to a specific platform even when the user's target source is not platform-specific -> reason from source type, not brand habit.

Related:
- `systems/constraints/rules/web-search/00-INDEX.md`
- `systems/constraints/rules/web-search/active/WEB-010-web-search-strategy.md`
- `systems/constraints/rules/web-search/active/WEB-011-query-variant-strategy.md`
- `systems/constraints/constraints/web-search/settings.json`
