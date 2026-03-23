# systems/constraints/rules/exec-tool/active/EXEC-025-exec-host-security-approvals.md

Title: Exec Host, Security, and Approvals

ID: EXEC-025
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-02-25

When:
- Before running `functions.exec` on `host=gateway` or `host=node`.
- When a command is denied, stuck pending approval, or behaves differently across `sandbox/gateway/node`.
- When diagnosing approval prompts (`ask`) and allowlist enforcement (`security`).

Rule:
- MUST choose the exec target (`host`) and understand its security model before attempting PATH or wrapper changes.
- MUST NOT assume `host=sandbox` implies isolation; sandboxing/isolation is a separate capability and may be disabled.
- MUST treat the effective exec policy as the stricter combination of:
  - tool-level defaults/config under `tools.exec.*`, and
  - host approvals policy (allowlist / approvals) for `gateway`/`node` execution.
- SHOULD record both the requested policy and the observed behavior (denied vs approval-pending vs allowed).
- When `security: "full"` is used because narrower runtime guard behavior (`allowlist`, approvals, or host gating) is unreliable or over-restrictive, the command MUST remain narrowly scoped to the task and MUST NOT be treated as a general expansion of execution authority (see `EXEC-055`).

Defaults (per OpenClaw docs; verify against your running version when debugging):
- `tools.exec.host` defaults to `sandbox`.
- `tools.exec.security` defaults to:
  - `deny` for sandbox
  - `allowlist` for gateway + node when unset
- `tools.exec.ask` defaults to `on-miss`.

Host-specific constraints and behavior:
- `host=gateway` / `host=node`:
  - `env.PATH` overrides are rejected.
  - Loader overrides (`LD_*` / `DYLD_*`) are rejected.
  - Approvals for host execution are controlled by `~/.openclaw/exec-approvals.json`.
- `host=node`:
  - Node hosts ignore `PATH` overrides (and `tools.exec.pathPrepend` does not apply to nodes).
  - If you need additional PATH entries on a node, configure the node host service environment or install tools in standard locations.
- `host=sandbox`:
  - When sandboxing is enabled, exec runs `sh -lc` inside the container; `/etc/profile` may reset PATH.
  - OpenClaw prepends `env.PATH` after profile sourcing via an internal env var (no shell interpolation).
  - `tools.exec.pathPrepend` applies to gateway + sandbox only.

Escalation / approvals flow:
- When approvals are required, exec MAY return `status: "approval-pending"` immediately.
- Once approved (or denied/timed out), the gateway emits system events (e.g., `Exec finished`, `Exec denied`).

Rationale:
- Most "why did this run / why was it denied / why no prompt" issues are policy issues, not shell issues.
  A dedicated rule makes approvals, defaults, and host-specific restrictions the first-class troubleshooting path.

Examples:
- Inspect configured defaults:
  - `openclaw config get tools.exec.host`
  - `openclaw config get tools.exec.security`
  - `openclaw config get tools.exec.ask`
- Show exec approvals policy file:
  - `zsh -lc 'ls -la ~/.openclaw/exec-approvals.json'`

Failure Modes:
- "I used sandbox so it should be isolated" -> isolation may be off; verify sandboxing status; use gateway approvals or enable sandboxing.
- "Why no approval prompt?" -> check `tools.exec.ask` and effective policy; sandbox execution may not trigger approvals.
- "PATH changes have no effect on node" -> node ignores PATH overrides; install tools or configure node host environment.
- A rejected explicit `host` parameter MUST be distinguished from tool-policy denial, approval-gate failure, elevated gating failure, or sandbox unavailability; do not diagnose these as the same problem (see `EXEC-035`).

Related:
- [EXEC-010] Wrapper Selection
- [EXEC-020] PATH & Binary Resolution
- [EXEC-030] Where To Run
- `systems/constraints/rules/exec-tool/00-INDEX.md`

References:
- OpenClaw docs: `docs/tools/exec.md`
- OpenClaw docs: `docs/tools/exec-approvals.md`
