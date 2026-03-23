Title: Install Authorization

ID: SKILL-040
Status: active
Pack: skills-management
Owner: user
Last Updated: 2026-03-09

When:
- The agent is deciding whether a skill installation may proceed.

Rule:
- MUST obtain the appropriate installation authorization before installing a skill.
- MUST NOT silently install unreviewed third-party skills.
- MUST require explicit confirmation for high-risk candidate skills.
- MUST ensure that authorization happens after the pre-install security report, not before it.
- MUST NOT use lower-friction install behavior unless the source trust is high, residual risk is low, and the user intent to install is already explicit.

Rationale:
- Installation changes the local capability surface and trust boundary.
- The user should authorize installation after seeing the relevant risks, not before.

Examples:
- High-risk third-party skill -> provide security report, then ask whether to proceed.
- Trusted low-risk skill with explicit user install request -> lower-friction path may be acceptable, but only after reporting.

Failure Modes:
- The agent treats interest in a skill as install authorization -> violation.
- The agent asks for authorization before presenting security findings -> violation.

Related:
- `systems/constraints/rules/skills-management/00-INDEX.md`
- `systems/constraints/rules/skills-management/active/30-pre-install-security-report.md`
- `systems/constraints/rules/skills-management/active/50-install-scope-defaults.md`
