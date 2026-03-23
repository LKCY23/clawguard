Title: Web Search Strategy

ID: WEB-010
Status: active
Pack: web-search
Owner: user
Last Updated: 2026-03-01

When:
- The user asks to search the web, find sources, look up information, or gather links.
- The agent needs external context to answer accurately.

Rule:
- MUST use MCP SearXNG (mcporter server `searxng`) as the primary web search backend.
- MUST perform search using `searxng.searxng_web_search`.
- MUST use `searxng.web_url_read` for follow-up reading when snippets are insufficient.
- MUST cap follow-up reads to 5 URLs per user request unless the user explicitly asks for deeper research.
- MUST include source URLs for factual claims.

- SHOULD do one query rewrite when search returns no results:
  - remove extra modifiers (e.g., "教授", "官网", "论文")
  - switch language terms (Chinese <-> English)
  - add an organization/affiliation keyword if known

- MAY fallback to openclaw-managed browser + Google search ONLY when at least one fallback condition is met:
  - MCP SearXNG returns 0 results after one query rewrite
  - MCP SearXNG or the SearXNG instance is unreachable (timeouts / 5xx / transport errors)
  - target sites require browser rendering / JS interaction and `web_url_read` cannot fetch meaningful content

- MUST explain the reason for fallback in one sentence when using browser fallback.

Rationale:
- MCP SearXNG is faster, more reproducible, and keeps search/read operations tool-driven.
- Browser fallback is heavier but more robust for dynamic pages and edge cases.

Examples:
- Primary search:
  - `searxng.searxng_web_search(query="openclaw", language="zh")`
  - Pick 1-3 results -> `searxng.web_url_read(url="https://...", maxLength=4000)`
- Fallback:
  - Start `profile="openclaw"` browser, navigate to Google, search query, summarize top results.

Failure Modes:
- SearXNG returns no results -> rewrite query once -> still empty -> fallback to browser.
- `web_url_read` returns navigation text only -> fallback to browser for that URL or choose a different source.

Related:
- `systems/constraints/rules/web-search/00-INDEX.md`
- `systems/constraints/registry/00-INDEX.md`

Overrides: (optional)
- None.
