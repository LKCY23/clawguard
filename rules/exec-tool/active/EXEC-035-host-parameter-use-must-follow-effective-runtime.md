Title: Host Parameter Use Must Follow Effective Runtime

ID: EXEC-035
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-03-10

When:
- The agent is about to call `functions.exec` and needs to decide whether to set the `host` parameter.
- The current session has an effective execution runtime that can be inferred from trusted runtime state, sandbox inspection, or repeated observed runtime behavior during the current environment.
- The task could run on more than one execution target (`sandbox`, host/gateway, or `node`), or the agent is considering an explicit host override.

Rule:
- The agent MUST determine the effective execution runtime before forcing an explicit `host` on `functions.exec`.
- The agent SHOULD treat the effective runtime as more authoritative than assumptions derived from static config snippets alone.
- When the effective runtime already resolves the session to direct host execution, the agent SHOULD NOT set `host` explicitly unless there is a verified need to override the default route.
- When the effective runtime resolves the session to sandboxed execution, the agent MAY rely on the default sandbox route and SHOULD avoid forcing a different host unless the task requires it and the relevant gates are satisfied.
- The agent MAY set `host` explicitly only when at least one of the following is true:
  - the task must run on a connected paired `node`
  - the current tool bridge/runtime has been verified to support explicit host selection for the chosen target
  - a higher-priority rule or explicit user instruction requires a specific execution target
- The agent MUST NOT assume that documentation-level execution semantics and tool-call parameter behavior are identical in every runtime or bridge.
- If an explicit `host` attempt fails, the agent MUST diagnose the failure at the correct layer before concluding that `exec` is unavailable.
- At minimum, the agent MUST distinguish among:
  - effective runtime says sandbox is off, but the bridge rejects an explicit sandbox target
  - effective runtime says host/direct, but the bridge rejects an explicit gateway/host target
  - `exec` itself is denied by tool policy
  - sandbox runtime is unavailable
  - elevated/approval gates are blocking an override path
- Host routing and wrapper selection are separate decisions. The agent SHOULD continue to apply the wrapper-selection rule independently (for example, prefer `zsh -lc '<cmd>'` where appropriate).

Rationale:
- OpenClaw separates sandbox mode, tool policy, and elevated behavior, but these layers do not always map one-to-one onto every tool bridge's accepted parameters.
- A session can effectively run on the host while still rejecting an explicitly forced `host` value at the tool-call layer.
- If the agent confuses execution semantics with parameter semantics, it will misdiagnose failures and make fragile host-routing choices.
- Basing host selection on effective runtime first, and only then on verified bridge behavior, reduces avoidable failures and keeps execution aligned with actual system behavior.

Examples:
- Good:
  - Trusted runtime state shows `runtime: direct` and `mode: off`; the agent calls `functions.exec` without `host`, using `command: "zsh -lc 'pwd'"`.
- Good:
  - A task must run on a connected paired device; the agent explicitly sets `host: "node"` with the intended node id.
- Good:
  - The agent verifies that the current runtime supports explicit host selection for a required path, then sets `host` intentionally.
- Bad:
  - The agent reads that sandbox-off execution semantically lands on host, then forces `host: "sandbox"` without verifying that the current bridge accepts that parameter.
- Bad:
  - The agent sees a rejected explicit `host` and concludes that `exec` is disabled, without checking tool policy, runtime mode, or bridge behavior.
- Bad:
  - The agent treats `host` selection and shell-wrapper selection as the same decision and changes both at once without diagnosing which layer actually matters.

Failure Modes:
- Static config is treated as the whole truth, while effective runtime state is ignored.
- Documentation semantics are applied too literally to tool-call parameters.
- The agent hard-codes explicit `host` values in sessions where the default route is already correct.
- A bridge-level rejection of explicit host selection is misreported as a general `exec` failure.
- The agent uses `host` overrides to compensate for problems that actually belong to tool policy, approvals, sandbox availability, or shell environment setup.

Related:
- `systems/constraints/rules/exec-tool/00-INDEX.md`
- `systems/constraints/rules/exec-tool/active/EXEC-010-wrapper-selection.md`
- `systems/constraints/rules/exec-tool/active/EXEC-025-exec-host-security-approvals.md`
- `systems/constraints/rules/exec-tool/active/EXEC-030-where-to-run.md`
- `systems/constraints/CONSTRAINTS_SYSTEM.md`
