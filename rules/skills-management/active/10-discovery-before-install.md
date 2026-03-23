Title: Discovery Before Install

ID: SKILL-010
Status: active
Pack: skills-management
Owner: user
Last Updated: 2026-03-09

When:
- The agent is searching for a skill, listing skill options, or evaluating whether a skill should be installed.

Rule:
- MUST distinguish discovery from installation.
- MUST identify the user's actual need before recommending installation.
- MUST consider whether an already-installed skill or built-in capability already covers the need.
- MUST NOT treat the existence of a candidate skill as sufficient reason to install it.
- MUST recommend discovery-only output when installation is not yet justified.

Rationale:
- Skill discovery is cheap; skill installation changes the system state and trust surface.
- Separating discovery from installation reduces impulsive or unnecessary installs.

Examples:
- A user asks whether a skill exists for a task -> list candidate skills first; do not install by default.
- A user asks to install a skill -> still confirm the task fit before proceeding to install review.

Failure Modes:
- The agent finds a plausible skill and immediately installs it -> violation; discovery does not imply install.
- The agent ignores an already-installed equivalent capability -> stop and re-check current skills first.

Related:
- `systems/constraints/rules/skills-management/00-INDEX.md`
- `systems/constraints/rules/skills-management/active/20-source-trust-classification.md`
