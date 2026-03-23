# systems/constraints/rules/exec-tool/active/EXEC-020-path-and-binary-resolution.md

Title: PATH & Binary Resolution

ID: EXEC-020
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-02-25

When:
- Any time a command fails due to PATH differences or missing binaries.
- Any time the agent needs to call system binaries that may live outside minimal PATH (e.g., `/usr/sbin/*`).

Rule:
- MUST prefer absolute paths for known system utilities when PATH is suspect.
- MUST use the following retry order for `command not found`:
  1) Retry via an available login-shell wrapper (see [EXEC-010])
  2) Retry using an absolute path (if known)
  3) Probe the resolved path using `command -v <name>` under a login-shell wrapper
- MUST NOT treat “it works in my terminal” as sufficient evidence; MUST capture evidence via probes.
- SHOULD record the exact failing command and the successful retry variant.
- SHOULD be aware that some environments restrict PATH overrides (e.g., host may reject `env.PATH` overrides; some nodes may discard PATH overrides).

Rationale:
- A deterministic retry/probe sequence reduces time-to-fix and prevents random “just try more things” behavior.

Examples:
- Prefer absolute path (macOS):
  - `/usr/sbin/screencapture -x ./deploy-screen.png`
- Probe a binary under login shell:
  - `zsh -lc 'command -v screencapture || true'`
  - `bash -lc 'command -v screencapture || true'`

Failure Modes:
- Tool exists but not in PATH -> use absolute path; do not guess.
- PATH overrides do not apply -> use absolute paths and/or wrapper; do not rely on `env.PATH`.

Related:
- [EXEC-010] Wrapper Selection
- [EXEC-040] Screenshots
- `systems/constraints/rules/exec-tool/00-INDEX.md`
