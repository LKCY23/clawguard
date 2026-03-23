Title: Post-Install Reporting

ID: SKILL-060
Status: active
Pack: skills-management
Owner: user
Last Updated: 2026-03-09

When:
- A skill installation, update, or removal has completed.

Rule:
- MUST provide a post-action report after skill lifecycle changes complete.
- MUST include at least:
  - skill name
  - source
  - install scope
  - target path
  - whether OpenClaw recognizes the skill
  - residual risks or important caveats
- MUST summarize previously reported security findings when they materially affect the installed result.
- MUST NOT use the post-install report as the first disclosure of critical security information.

Rationale:
- After a lifecycle change, the user needs a concise record of what changed and what still matters.
- Post-action reporting closes the loop without replacing pre-install risk review.

Examples:
- After install: report path, scope, recognition status, and remaining caveats.
- After update: report what changed and whether any risk posture changed.

Failure Modes:
- The user cannot tell where the skill was installed or whether OpenClaw sees it -> incomplete reporting.
- Security concerns appear for the first time only in the post-install report -> violation.

Related:
- `systems/constraints/rules/skills-management/00-INDEX.md`
- `systems/constraints/rules/skills-management/active/30-pre-install-security-report.md`
- `systems/constraints/rules/skills-management/active/50-install-scope-defaults.md`
