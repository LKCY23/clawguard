# systems/constraints/constraints/skills-management/00-INDEX.md

Pack:
- skills-management

Purpose:
- Provide routing + compliance checks for skill discovery, review, installation, update, removal, and reporting.
- Coordinate safe use of `find-skills`, `npx skills ...`, and other skill-management flows.

Triggers (examples):
- User asks to find a skill.
- User asks to install/update/remove a skill.
- Agent is about to use `find-skills` or `npx skills ...`.

Minimal reads:
- MUST load the skills-management rules pack entrypoint: `systems/constraints/rules/skills-management/00-INDEX.md`.

Escalation policy:
- Within this pack, the agent MAY load any file listed under "Escalation reads" without additional user consent.
- The agent MUST keep escalation reads minimal (pick the smallest applicable set).
- Any cross-pack escalation requires explicit user consent unless mandated by `systems/constraints/CONSTRAINTS_SYSTEM.md`.

Escalation reads (pick the smallest applicable set):
- Discovery/install separation: `systems/constraints/rules/skills-management/active/10-discovery-before-install.md`
- Source trust classification: `systems/constraints/rules/skills-management/active/20-source-trust-classification.md`
- Installer/tool security signals: `systems/constraints/rules/skills-management/active/25-installer-signal-security-review.md`
- Local content review: `systems/constraints/rules/skills-management/active/26-local-skill-content-review-before-install.md`
- Pre-install security report: `systems/constraints/rules/skills-management/active/30-pre-install-security-report.md`
- Install authorization: `systems/constraints/rules/skills-management/active/40-install-authorization.md`
- Install scope defaults: `systems/constraints/rules/skills-management/active/50-install-scope-defaults.md`
- Post-install reporting: `systems/constraints/rules/skills-management/active/60-post-install-reporting.md`

Checks (reference-first):
- Verify applicable constraints in `systems/constraints/rules/skills-management/active/10-discovery-before-install.md` before treating discovery as install intent.
- Verify applicable constraints in `systems/constraints/rules/skills-management/active/20-source-trust-classification.md` before installation.
- Verify applicable constraints in `systems/constraints/rules/skills-management/active/25-installer-signal-security-review.md` when installer security output exists.
- Verify applicable constraints in `systems/constraints/rules/skills-management/active/26-local-skill-content-review-before-install.md` before installation.
- Verify applicable constraints in `systems/constraints/rules/skills-management/active/30-pre-install-security-report.md` before seeking install authorization or executing install.
- Verify applicable constraints in `systems/constraints/rules/skills-management/active/40-install-authorization.md` before installation.
- Verify applicable constraints in `systems/constraints/rules/skills-management/active/50-install-scope-defaults.md` when choosing workspace vs global scope.
- Verify applicable constraints in `systems/constraints/rules/skills-management/active/60-post-install-reporting.md` after install/update/remove actions complete.
- Reporting receipt (include in the user-visible response for skill lifecycle actions):
  - Applied rules: SKILL-010[, SKILL-020][, SKILL-025][, SKILL-026][, SKILL-030][, SKILL-040][, SKILL-050][, SKILL-060]
  - Checks performed: need/fit review; source classification; installer-signal review when available; local content review; pre-install security report; install authorization; scope selection; post-install reporting

Authoritative references:
- `systems/constraints/rules/skills-management/00-INDEX.md`
- `systems/constraints/registry/skills-management.yml`
