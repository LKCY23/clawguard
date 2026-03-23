# systems/constraints/rules/README.md

This directory contains rule packs governed by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Quick Nav:
- `artifacts` — temporary artifacts (snapshots, generated outputs, TTL cleanup)
  - Entry: `systems/constraints/rules/artifacts/00-INDEX.md`
- `docker-control` — Docker daemon / container / image / network / volume control
  - Entry: `systems/constraints/rules/docker-control/00-INDEX.md`
- `exec-tool` — how the agent should use `functions.exec` / `functions.process`
  - Entry: `systems/constraints/rules/exec-tool/00-INDEX.md`
- `repo-hygiene` — workspace persistence + git hygiene (write/commit/push)
  - Entry: `systems/constraints/rules/repo-hygiene/00-INDEX.md`
- `maintenance` — periodic checks (e.g., update reminders)
  - Entry: `systems/constraints/rules/maintenance/00-INDEX.md`
- `rules-governance` — governance of rules + registry registration workflow
  - Entry: `systems/constraints/rules/rules-governance/00-INDEX.md`
- `web-search` — how the agent should do web search + follow-up reading
  - Entry: `systems/constraints/rules/web-search/00-INDEX.md`

## Directory Conventions

Each pack lives at `systems/constraints/rules/<pack>/` and SHOULD contain:
- `00-INDEX.md` (thin index: purpose, non-goals, entry decision tree, terms, decision order, links)
- `templates/RULE_TEMPLATE.md`
- `active/` (default-enforced rules)
- `draft/` (proposed rules; not default)
- `deprecated/` (replaced rules; keep references)

## Add A New Rule (3 steps)

1) Copy the template
- Copy `systems/constraints/rules/<pack>/templates/RULE_TEMPLATE.md` into `systems/constraints/rules/<pack>/draft/`
- Choose a new ID: `<PACK>-<NNN>` (IDs MUST NOT be reused)

2) Write the rule
- Keep it small; split if it exceeds ~120 lines or examples exceed 6
- Use ALL CAPS normative keywords consistently (BCP 14 / RFC 2119 + RFC 8174)

3) Update the pack index
- Add a link to `systems/constraints/rules/<pack>/00-INDEX.md` using this format:
  - `- [<ID>] Title — one line description`

## Review & Activation

- Draft rules MUST be reviewed by the user before activation.
- After approval, move the file into `systems/constraints/rules/<pack>/active/` and update the pack index.
- In SHARED contexts, do not write to disk; provide suggestions/patch summaries only.
