# systems/constraints/rules/skills-management/00-INDEX.md

Purpose:
- Define rules for discovering, reviewing, installing, updating, removing, and reporting on agent skills.
- Coordinate safe use of `find-skills`, `npx skills ...`, and other skill-management flows.

Non-goals / Out of scope:
- The internal business logic of a specific skill after it is already trusted and installed.
- General shell safety outside skill lifecycle work (see `systems/constraints/rules/exec-tool/`).
- Generic repository persistence rules outside skill lifecycle work (see `systems/constraints/rules/repo-hygiene/`).

Entry Decision Tree (navigation-first):
- Are you only discovering/searching for skills?
  - Follow SKILL-010 and SKILL-020 before considering install.
- Are you preparing to install a skill?
  - Follow SKILL-020, SKILL-025, SKILL-026, SKILL-030, SKILL-040, then SKILL-050 and SKILL-060.
- Are you updating or removing a skill?
  - Apply the same source/risk and reporting discipline unless the skill is already fully trusted and the user has explicitly approved the lifecycle action.

Terms:
- skill: a reusable capability package organized around a `SKILL.md` entrypoint.
- discovery: finding candidate skills without installing them.
- source trust: the trust classification of a skill source (official, known third-party, unreviewed third-party, local path, etc.).
- installer signal: security/risk output produced by a skill-management tool during discovery or install.
- local content review: direct review of `SKILL.md` and related local files before install.
- pre-install security report: the user-visible consolidated safety summary shown before installation.
- workspace install: install into the current workspace skill directory.
- global install: install into the user's cross-project/global skill directory.

Decision Order (within this pack):
1) Safety and informed review before convenience
2) Pre-install reporting before install action
3) Workspace-local scope before broader scope
4) Traceability and clear post-install reporting

Active Rules (default-enforced):
- [SKILL-010] Discovery Before Install — Finding a skill does not justify installing it.
- [SKILL-020] Source Trust Classification — Classify skill sources and use risk signals before install.
- [SKILL-025] Installer-Signal Security Review — Surface installer/tool security signals before install.
- [SKILL-026] Local Skill Content Review Before Install — Review `SKILL.md` and related files before install.
- [SKILL-030] Pre-Install Security Report — Give a consolidated safety report before install.
- [SKILL-040] Install Authorization — Require the right confirmation level before installation.
- [SKILL-050] Install Scope Defaults — Prefer workspace installs; treat global installs as higher-scope actions.
- [SKILL-060] Post-Install Reporting — Report install results and residual risk after completion.

Draft Rules (not default):
- (none)

Notes:
- If rules in this pack conflict, apply this pack’s Decision Order first; if conflict remains, apply `systems/constraints/CONSTRAINTS_SYSTEM.md`.
