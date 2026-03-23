Title: Install Scope Defaults

ID: SKILL-050
Status: active
Pack: skills-management
Owner: user
Last Updated: 2026-03-09

When:
- The agent is choosing where a skill should be installed.

Rule:
- MUST prefer workspace installation by default.
- MUST treat global installation as a higher-scope action than workspace installation.
- MUST reserve global installation for mature, broadly reusable, cross-project skills unless the user explicitly chooses otherwise after review.
- MUST state the chosen scope before installation.
- MUST NOT expand installation scope casually or implicitly.

Rationale:
- Workspace installation keeps iteration local and limits blast radius.
- Global installation affects future work across contexts and therefore deserves a higher bar.

Examples:
- A new or experimental skill -> install into the workspace first.
- A stable utility skill used across many projects -> consider global scope after trust is established.

Failure Modes:
- The agent defaults to global because it seems convenient -> violation.
- The agent does not report install scope before installation -> violation.

Related:
- `systems/constraints/rules/skills-management/00-INDEX.md`
- `systems/constraints/rules/skills-management/active/40-install-authorization.md`
- `systems/constraints/rules/skills-management/active/60-post-install-reporting.md`
