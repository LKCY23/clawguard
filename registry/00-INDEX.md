# systems/constraints/registry/00-INDEX.md

This directory contains On-Demand Files Registry entries defined by the schema in `AGENTS.md`.

Purpose:
- Provide a stable, lightweight routing layer from user intent -> domain -> minimal reads.
- Keep `AGENTS.md` as a framework, not a mega-list of file paths.

How to use:
- When extra local context is required, read this index first, select a matching entry by `triggers`,
  then read only that entry's `minimalRead*` list.
- Escalation reads are allowed only when required and only with explicit user consent.

Entries:
- `exec.yml` — pack: exec-tool (execution/tooling: `functions.exec`, `functions.process`)
- `repo.yml` — pack: repo-hygiene (workspace persistence + git hygiene: write/commit/push)
- `web-search.yml` — pack: web-search (primary MCP SearXNG, fallback to managed browser)
- `rules-governance.yml` — pack: rules-governance (RULES_SYSTEM + rules packs)
- `artifacts.yml` — pack: artifacts (snapshots, generated outputs, TTL cleanup)
- `maintenance.yml` — pack: maintenance (update checks/reminders)
- `messaging.yml` — pack: messaging (outbound messaging: send/reply/edit/delete)
- `docker.yml` — pack: docker-control (Docker daemon / container / image / volume / network control)
- `ssh-host-admin.yml` — pack: ssh-host-admin (SSH access to host terminal with strict scope limits)
- `skills-management.yml` — pack: skills-management (skill discovery, trust review, install/update/remove lifecycle)
