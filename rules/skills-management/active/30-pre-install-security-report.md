Title: Pre-Install Security Report

ID: SKILL-030
Status: active
Pack: skills-management
Owner: user
Last Updated: 2026-03-09

When:
- The agent is about to install a skill.

Rule:
- MUST provide a consolidated security report before installation.
- MUST include, when available:
  - source trust classification
  - installer-provided security signals
  - local content review findings
  - residual risks and unknowns
- MUST present the report before installation authorization is sought or installation begins.
- MUST NOT first disclose critical security concerns only after installation.
- MUST make clear what the report does and does not prove.

Rationale:
- Security review only helps when it informs the install decision before system state changes.
- Consolidating the report reduces the risk that important warnings are scattered or buried.

Examples:
- A skill with low public trust signals and a script that edits config silently -> report both signals before asking whether to proceed.
- A trusted source with low residual risk -> report that clearly before installation.

Failure Modes:
- The agent asks for install confirmation without first giving the security report -> violation.
- The agent gives partial safety signals before install and only reveals the worst risk after install -> violation.

Related:
- `systems/constraints/rules/skills-management/00-INDEX.md`
- `systems/constraints/rules/skills-management/active/20-source-trust-classification.md`
- `systems/constraints/rules/skills-management/active/25-installer-signal-security-review.md`
- `systems/constraints/rules/skills-management/active/26-local-skill-content-review-before-install.md`
- `systems/constraints/rules/skills-management/active/40-install-authorization.md`
