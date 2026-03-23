Title: Installer-Signal Security Review

ID: SKILL-025
Status: active
Pack: skills-management
Owner: user
Last Updated: 2026-03-09

When:
- The agent uses a skill-management tool or installer that emits security, trust, or risk review output before installation.

Rule:
- MUST surface installer/tool security review output before installation.
- MUST present, when available:
  - check names
  - risk ratings or summaries
  - relevant detail links
  - a short interpretation in plain language
- MUST NOT bury installer security signals only in raw logs.
- MUST NOT postpone first disclosure of installer safety signals until after installation.
- MUST treat installer signals as review inputs, not final safety proof.

Rationale:
- Installer tools may provide useful trust and supply-chain signals.
- Those signals only help if the user sees them before deciding whether installation should proceed.

Examples:
- A skills installer emits package risk ratings -> show them before asking whether to proceed.
- A detail page exists -> include the link in the pre-install report.

Failure Modes:
- The installer showed risk output but the agent only reports it after install -> violation.
- The agent copies a risk label without explaining its limits -> incomplete review.

Related:
- `systems/constraints/rules/skills-management/00-INDEX.md`
- `systems/constraints/rules/skills-management/active/20-source-trust-classification.md`
- `systems/constraints/rules/skills-management/active/30-pre-install-security-report.md`
