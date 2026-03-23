Title: Source Trust Classification

ID: SKILL-020
Status: active
Pack: skills-management
Owner: user
Last Updated: 2026-03-09

When:
- The agent is evaluating a skill source before installation, update, or other lifecycle action.

Rule:
- MUST classify the skill source before installation.
- MUST distinguish at least: official/high-trust sources, known third-party sources, unreviewed third-party sources, and local-path sources.
- MUST use public trust signals such as star count and install count as supporting evidence when they are available.
- MUST treat `stars < 250` or `installs < 1500` as high-risk candidate signals by default.
- MUST NOT treat trust signals, popularity, or installer metadata as proof of safety.
- MUST route high-risk candidates into stricter review and non-silent install behavior.

Rationale:
- Skill sources are not equally trustworthy.
- Popularity signals can help prioritize review but cannot replace real safety judgment.

Examples:
- A low-star, low-install third-party skill -> classify as high-risk candidate.
- A local path from the user's own workspace -> classify separately from external third-party sources.

Failure Modes:
- The agent treats any GitHub-hosted skill as effectively trusted -> violation; hosting location is not trust proof.
- The agent uses popularity as a substitute for review -> violation; signals inform risk, they do not prove safety.

Related:
- `systems/constraints/rules/skills-management/00-INDEX.md`
- `systems/constraints/rules/skills-management/active/25-installer-signal-security-review.md`
- `systems/constraints/rules/skills-management/active/30-pre-install-security-report.md`
