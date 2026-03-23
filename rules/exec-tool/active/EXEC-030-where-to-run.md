# systems/constraints/rules/exec-tool/active/EXEC-030-where-to-run.md

Title: Where To Run (sandbox vs gateway vs node)

ID: EXEC-030
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-02-25

When:
- Before running any command via `functions.exec`.

Rule:
- MUST choose the execution target (`host`: `sandbox` | `gateway` | `node`) before choosing wrapper/path tweaks.
- SHOULD default to the configured OpenClaw exec host (see `tools.exec.host`) unless there is a clear reason to override.
- MUST NOT treat `sandbox` as a guarantee of isolation; isolation is a separate capability and may be disabled.
- SHOULD prefer `node` when the task depends on a user’s logged-in desktop/session, device-local tools, or GUI state,
  and the node is available (`connected=true`).
- SHOULD use `gateway` when you need host execution semantics (approvals, allowlist enforcement) or when sandboxing is unavailable.
- MUST NOT assume node capabilities exist unless `connected=true` and the needed caps/commands are available.
- Choosing the effective execution target does not, by itself, imply that `host` should be forced explicitly at tool-call time; explicit host selection MUST follow verified effective runtime and bridge behavior (see `EXEC-035`).

Rationale:
- Many “it doesn’t work” issues come from running in the wrong place, or from assuming sandbox implies isolation.
  Choosing the right target first reduces permissions friction and avoids unsafe hacks.

Examples:
- Use configured defaults (no explicit host override):
  - `functions.exec(command="<cmd>")`
- Force host execution on gateway:
  - `functions.exec(host="gateway", command="<cmd>")`
- Run on a node:
  - `functions.exec(host="node", node="<node-id-or-name>", command="<cmd>")`

Failure Modes:
- `node not connected` -> do not keep retrying; connect the node or choose `gateway`/`sandbox` alternative.
- Approvals confusion -> read [EXEC-025] and verify `tools.exec.host/security/ask` + `~/.openclaw/exec-approvals.json`.

Related:
- [EXEC-025] Exec Host, Security, and Approvals
- [EXEC-010] Wrapper Selection
- `systems/constraints/rules/exec-tool/00-INDEX.md`
