# systems/constraints/rules/exec-tool/00-INDEX.md

Purpose:
- Define how the agent uses `functions.exec` and `functions.process` in this workspace.

Non-goals / Out of scope:
- Browser automation (`functions.browser`)
- Device/node operations (`functions.nodes`) beyond selecting the correct run target
- Messaging (`functions.message`)
- General security policy beyond exec-specific safety constraints

Entry Decision Tree (navigation-first):
- Choose where to run (sandbox vs gateway/host vs node)
- Check effective security/approval policy (tools.exec + approvals)
- Choose shell wrapper (platform-appropriate; prefer login shell when available)
- Prefer absolute paths for system binaries when PATH is suspect
- Decide PTY/background/process strategy
- Troubleshoot (probe `PATH`, `command -v`, retry order)
- If the command changes durable local state, daemon behavior, or future runtime behavior: emit a short preflight receipt before execution.

Terms:
- host/gateway: the gateway host machine where OpenClaw runs
- sandbox: an execution target; isolation may be disabled unless explicitly enabled
- node: a paired device execution target (requires connected=true)
- login shell: a shell started in login mode (e.g., `zsh -l`) that loads login initialization files
- wrapper: the shell invocation used to run a command (e.g., `zsh -lc '<cmd>'`)
- absolute path: invoking a binary by full path (e.g., `/usr/sbin/screencapture`)
- PTY: pseudo-terminal; required for some interactive CLIs
- background: run a command asynchronously and interact via `functions.process`

Decision Order (within this pack):
1) Safety constraints (must not run destructive commands implicitly; avoid secret leakage)
2) Where to run (sandbox/gateway/node)
3) Effective security + approvals (tools.exec + approvals; stricter policy wins)
4) Wrapper choice (prefer login shell when available; fall back when not)
5) PATH / binary resolution (absolute paths when needed)
6) Long-running execution mechanics (pty/background/process)
7) Reporting format for outcomes and failures

Active Rules (default-enforced):
- [EXEC-010] Wrapper Selection — Choose an available login-shell wrapper (zsh/bash/sh) based on platform
- [EXEC-020] PATH & Binary Resolution — Avoid `command not found` via retry/probe order
- [EXEC-025] Exec Host, Security, and Approvals — Defaults, effective policy merge, and troubleshooting
- [EXEC-030] Where To Run — sandbox vs gateway vs node selection rules
- [EXEC-035] Host Parameter Use Must Follow Effective Runtime — Force `host` only when the effective runtime and current bridge behavior support it
- [EXEC-040] Screenshots — Prefer `/usr/sbin/screencapture`, fallback to `ffmpeg` avfoundation; handle GUI constraints
- [EXEC-050] Long-Running & PTY — Background sessions, PTY usage, defaults, and cleanup
- [EXEC-055] Platform Guard Drift Requires Agent Self-Restraint — When runtime guards are unreliable, fall back to rule-based execution restraint
- [EXEC-060] Exec Safety — Destructive commands, repo hygiene, and secret handling
- [EXEC-070] Reporting — What to report on success/failure
- [EXEC-080] Stateful Exec Actions Require Preflight Receipt — Config/daemon/runtime-changing exec actions need a pre-action self-check

Notes:
- If rules in this pack conflict, apply this pack’s Decision Order first; if conflict remains, apply the
  global conflict resolution order (see `systems/constraints/CONSTRAINTS_SYSTEM.md`).
