# systems/constraints/rules/maintenance/00-INDEX.md

Purpose:
- Define rules for periodic maintenance checks (version updates, health checks) and how the agent should notify the user.

Non-goals / Out of scope:
- How to perform upgrades/changes themselves (handled by user authorization and `repo-hygiene` / safety rules).
- Detailed security hardening (separate pack).

Entry Decision Tree (navigation-first):
- Is this a periodic check request?
  - If it is an update check: follow MAINT-010.

Terms:
- update check: comparing installed versions to upstream latest.
- remind: notify the user with current vs latest and how to update.
- upgrade: actually installing a new version.

Decision Order (within this pack):
1) Safety and user agency (user decides upgrades)
2) Correctness (accurate version comparison)
3) Low-noise notifications

Active Rules (default-enforced):
- [MAINT-010] OpenClaw Update Checks — Check daily and remind; never auto-upgrade
- [MAINT-020] Gateway Restart Handshake — Pre/post restart notices to preserve visibility

Draft Rules (not default):
- (none)
