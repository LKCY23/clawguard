# systems/constraints/rules/exec-tool/deprecated/EXEC-000-exec-tool-monolith.md

Title: Exec Tool Monolith (Legacy)

ID: EXEC-000
Status: deprecated
Pack: exec-tool
Owner: user
Last Updated: 2026-02-25

When:
- Reference only. This file is kept for historical context and migration traceability.

Rule:
- MUST NOT treat this file as the default source of truth for exec behavior.
- MUST use the active rules referenced below as the default-enforced policies.

Rationale:
- The workspace originally captured exec norms in a single document (`EXEC_TOOL.md`). As the rules system
  matured, this was split into small, evolvable rules (thin index + multiple active rules). Keeping the
  original monolith helps explain past decisions and provides provenance.

Replaced by (active rules):
- [EXEC-010] Wrapper Selection — `systems/constraints/rules/exec-tool/active/EXEC-010-wrapper-selection.md`
- [EXEC-020] PATH & Binary Resolution — `systems/constraints/rules/exec-tool/active/EXEC-020-path-and-binary-resolution.md`
- [EXEC-025] Exec Host, Security, and Approvals — `systems/constraints/rules/exec-tool/active/EXEC-025-exec-host-security-approvals.md`
- [EXEC-030] Where To Run — `systems/constraints/rules/exec-tool/active/EXEC-030-where-to-run.md`
- [EXEC-040] Screenshots — `systems/constraints/rules/exec-tool/active/EXEC-040-screenshots.md`
- [EXEC-050] Long-Running & PTY — `systems/constraints/rules/exec-tool/active/EXEC-050-long-running-and-pty.md`
- [EXEC-060] Exec Safety — `systems/constraints/rules/exec-tool/active/EXEC-060-exec-safety.md`
- [EXEC-070] Reporting — `systems/constraints/rules/exec-tool/active/EXEC-070-reporting.md`

References:
- Original source: `EXEC_TOOL.md` (pre-pack document)

---

# EXEC_TOOL.md (Original Content)

This document defines how the agent should use `functions.exec` (and related shell execution)
in this workspace, so behavior is consistent with the user's terminal expectations.

## 1) Scope

Applies to:
- Shell command execution via `functions.exec`
- Managing long-running commands via `functions.process`
- Diagnosing command failures caused by environment differences (PATH, login shell, etc.)

Does not replace:
- Browser automation (`functions.browser`)
- Node device commands (`functions.nodes`)
- Messaging (`functions.message`)

## 2) Defaults (Do This Unless There Is a Reason Not To)

- Default wrapper: run commands as a login shell:
  - `zsh -lc '<cmd>'`
- Rationale: align PATH and shell initialization closer to an interactive terminal, including
  standard system paths like `/usr/sbin`.

## 3) When NOT To Use `zsh -lc`

Use a plain command (no login shell wrapper) when:
- The command must run in a minimal/controlled environment for reproducibility
- Startup overhead matters (tiny commands in tight loops)
- The command explicitly relies on a custom env provided via `exec(env=...)`
- The command is already an absolute-path binary and PATH is irrelevant

If unsure, prefer `zsh -lc`.

## 4) PATH Policy (Avoid "command not found")

- Prefer absolute paths for system utilities that commonly live outside a minimal PATH:
  - Example: `/usr/sbin/screencapture` (do not rely on `/usr/sbin` being in PATH)
- If a command fails with `command not found`:
  1) Retry with `zsh -lc '<cmd>'`
  2) If still failing, retry using an absolute path (if known)
  3) If path is unknown, probe with `command -v <name>` (under `zsh -lc`)

## 5) Where To Run (host vs sandbox vs node)

- `host` (gateway host): default for most commands and local file operations.
- `sandbox`: use when you want isolation, reduced permissions, or to avoid touching host state.
- `node`: use when you need to act on a paired device (GUI state, user session, device-specific tools).
  Note: node actions require `connected=true`.

Rule of thumb:
- If the task depends on the user's logged-in desktop/session: prefer `node` (when available/connected).
- If the task is purely CLI + files in workspace: prefer `host`.

## 6) Screenshots / Screen Capture

Preferred order:
1) Use `/usr/sbin/screencapture` (absolute path).
2) Fallback to `/opt/homebrew/bin/ffmpeg` with `-f avfoundation` for screen capture.

### 6.1) `screencapture` (Preferred)

- Command pattern:
  - `/usr/sbin/screencapture -x <output.png>`
- Notes:
  - Use `-x` to suppress sounds.
  - Use absolute path to avoid PATH issues.

### 6.2) `ffmpeg` + avfoundation (Fallback)

- Device discovery pattern (example):
  - `/opt/homebrew/bin/ffmpeg -f avfoundation -list_devices true -i ""`
- Single-frame screenshot pattern (example):
  - `/opt/homebrew/bin/ffmpeg -f avfoundation -i "0" -frames:v 1 -y <output.png>`
- Notes:
  - Pixel format warnings may appear; capture can still succeed.
  - Prefer saving artifacts under the workspace, then send via `message` tool.

### 6.3) Output Conventions

- Default output directory:
  - `/Users/openclaw/.openclaw/workspace/`
- Recommended naming:
  - `deploy-screen.png`
  - `deploy-screen-YYYYMMDD-HHMMSS.png` (when multiple versions matter)

## 7) Long-Running Commands (PTY, background, process)

- Use `pty=true` when:
  - The command requires a TTY (interactive CLIs, password prompts, REPLs)
- Use `background=true` when:
  - The command takes longer than a normal turn and you need to poll logs/status via `functions.process`

Guideline:
- Prefer non-interactive commands whenever possible.
- Avoid leaving background sessions running indefinitely; explicitly kill when done.

## 8) Safety Rules

- Avoid destructive commands by default:
  - Do not run `rm -rf` unless explicitly requested and clearly scoped.
  - Prefer recoverable deletion where available (e.g., `trash`) or staged actions.
- Before making changes in repos:
  - Check status/diff first (`git status`, `git diff`) when relevant.
- Do not exfiltrate secrets:
  - Never print or forward API keys, tokens, or private keys.
  - Be careful with `env`, `printenv`, and logs.

## 9) Reporting Expectations

- For successful actions:
  - Provide concise confirmation + artifact location (file path) or send the artifact via `message`.
- For failures:
  - Include the raw error line(s) and the exact command used (or a redacted version if sensitive).
  - State the next best retry strategy (login shell vs absolute path vs node).

## 10) Change Log (Optional)

- 2026-02-25: Initial version. Default to `zsh -lc`. Prefer `/usr/sbin/screencapture`,
  fallback to `/opt/homebrew/bin/ffmpeg` + avfoundation.
