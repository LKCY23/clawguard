Title: Stateful Exec Actions Require Preflight Receipt

ID: EXEC-080
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-03-08

When:
- The agent is about to use `functions.exec` for an action that changes persistent local state.
- The agent is about to start/stop/restart a daemon or service.
- The agent is about to edit local configuration that changes later runtime behavior.
- The agent is about to run a command whose blast radius is higher than ordinary read-only probing.

Rule:
- MUST provide a short preflight self-check / receipt before executing such a command.
- MUST include at least:
  - intent
  - exact target
  - expected state change
  - verification plan
- MUST treat local config edits, daemon lifecycle actions, and persistent runtime changes as stateful exec actions even when they are scoped and reversible.
- MUST NOT use “it was just part of investigation” as a reason to skip the preflight receipt.
- This rule does NOT require waiting for user approval unless another rule or gate requires approval; it requires explicit self-check and operator-visible reporting before action.

Rationale:
- Stateful exec actions are easy to under-classify because they often look like ordinary shell work.
- A mandatory preflight receipt reduces hidden momentum and improves post-hoc reconstruction when something goes wrong.

Examples:
- Before running a daemon command:
  - `Intent: start mcporter daemon for keep-alive reuse; Target: mcporter daemon; Verify: mcporter daemon status`
- Before editing a config:
  - `Intent: enable keep-alive idle timeout; Target: ~/.mcporter/mcporter.json; Verify: mcporter config doctor`

Failure Modes:
- The agent jumps straight from diagnosis to a local config mutation without reporting -> violation.
- The agent reports only after execution -> insufficient unless clearly marked as backfilled after an accidental miss.

Related:
- [EXEC-060] Exec Safety
- [EXEC-070] Reporting
- `systems/constraints/CONSTRAINTS_RUNTIME.md`
- `systems/constraints/rules/exec-tool/00-INDEX.md`
