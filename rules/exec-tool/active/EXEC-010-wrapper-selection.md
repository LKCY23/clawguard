# systems/constraints/rules/exec-tool/active/EXEC-010-wrapper-selection.md

Title: Wrapper Selection

ID: EXEC-010
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-02-25

When:
- Any time the agent runs shell commands via `functions.exec` on sandbox/gateway/node.

Rule:
- MUST choose an available login-shell wrapper appropriate to the execution environment.
- SHOULD prefer a login shell to align PATH and initialization with an interactive terminal.
- MUST NOT assume `zsh` exists on non-macOS systems or on all nodes.
- For macOS gateway/host environments, SHOULD use `zsh -lc '<cmd>'` when `zsh` is available.
- If `zsh` is unavailable, SHOULD fall back to `bash -lc '<cmd>'`; if `bash` is unavailable, fall back to `sh -lc '<cmd>'`.
- MAY run a plain command (no wrapper) when at least one of the following is true:
  - The command must run in a minimal/controlled environment for reproducibility.
  - Startup overhead matters (tiny commands in tight loops).
  - The command explicitly relies on a custom env provided via `exec(env=...)`.
  - The command is already an absolute-path binary and PATH is irrelevant.

Rationale:
- Agent exec environments can differ from interactive terminals, commonly omitting system paths (e.g., `/usr/sbin`).
  A login-shell wrapper reduces `command not found` incidents and makes behavior more predictable.

Examples:
- Wrapper probing:
  - `zsh -lc 'command -v zsh bash sh | cat'`
- macOS gateway/host default wrapper:
  - `zsh -lc 'echo $PATH; command -v screencapture'`
- Fallback wrapper:
  - `bash -lc 'echo $PATH'`
  - `sh -lc 'echo $PATH'`

Failure Modes:
- `command not found: zsh` -> fall back to `bash -lc` then `sh -lc`.
- PATH differs from user terminal -> compare `sh -lc 'echo $PATH'` vs `zsh -lc 'echo $PATH'` (if available).

Related:
- [EXEC-020] PATH & Binary Resolution
- [EXEC-025] Exec Host, Security, and Approvals
- `systems/constraints/rules/exec-tool/00-INDEX.md`
