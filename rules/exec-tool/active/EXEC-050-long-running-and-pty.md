# systems/constraints/rules/exec-tool/active/EXEC-050-long-running-and-pty.md

Title: Long-Running Commands (PTY, background, process)

ID: EXEC-050
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-02-25

When:
- Any time a command may be interactive, long-running, or needs streaming output.

Rule:
- SHOULD prefer non-interactive commands whenever possible.
- MUST use `pty=true` when the command requires a TTY (interactive CLIs, prompts, REPLs).
- SHOULD use `background=true` for long-running commands and manage them via `functions.process`.
- MUST avoid leaving background sessions running indefinitely; MUST terminate sessions when finished.
- SHOULD set explicit `timeout` values when possible to prevent hung processes.

Rationale:
- PTY and background execution are powerful but create state and lifecycle concerns. Standardizing how to
  run and clean up long-running processes prevents resource leaks and confusing partial outputs.

Examples:
- Interactive command with PTY:
  - `functions.exec(command="<cmd>", pty=true)`
- Background via yield:
  - `functions.exec(command="<cmd>", yieldMs=1000)`
- Background immediately:
  - `functions.exec(command="<cmd>", background=true)`
- Poll logs:
  - `functions.process(action="poll", sessionId="<id>", timeout=5000)`

Failure Modes:
- Background behaves synchronously -> if `process` is disallowed, `exec` runs synchronously and ignores `yieldMs`/`background`.
- Cannot poll from another agent/session -> background sessions are scoped per agent; `process` only sees sessions from the same agent.
- Unexpected timeouts -> note defaults: `yieldMs` default is 10000ms; `timeout` default is 1800s (30m). Set explicitly when needed.

Related:
- [EXEC-030] Where To Run
- [EXEC-070] Reporting
- `systems/constraints/rules/exec-tool/00-INDEX.md`

References:
- OpenClaw docs: `docs/tools/exec.md`
