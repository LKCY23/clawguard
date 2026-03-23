# systems/constraints/rules/exec-tool/active/EXEC-060-exec-safety.md

Title: Exec Safety

ID: EXEC-060
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-02-25

When:
- Before running any potentially destructive or sensitive shell operation via `functions.exec`.

Rule:
- High-risk or state-changing commands MUST still require deliberate rule-based confirmation even when platform guard friction is reduced or absent (for example `ask: "off"` or `security: "full"`); platform permission does not replace agent safety judgment (see `EXEC-055`).
- MUST NOT run destructive commands (e.g., `rm -rf`, system config changes) unless explicitly requested
  by the user and clearly scoped.
- SHOULD prefer recoverable deletion when available (e.g., `trash`) over irreversible deletion.
- SHOULD check repository state before making changes when relevant:
  - `git status`, `git diff`
- MUST NOT exfiltrate secrets:
  - Never print or forward API keys, tokens, private keys, or credential blobs.
  - Be careful with commands that dump env or config (`env`, `printenv`, `cat ~/.ssh/*`, etc.).
- SHOULD redact sensitive strings in any user-visible output when uncertainty exists.

Rationale:
- Exec has the highest blast radius. A default safety posture prevents accidental data loss and leaks.

Examples:
- Prefer inspection before change:
  - `zsh -lc 'git status --porcelain'`
- Safer deletion (if `trash` exists):
  - `zsh -lc 'trash ./some-file'`

Failure Modes:
- Ambiguous destructive request -> ask one clarification question that narrows scope.
- Sensitive output appears -> stop and redact; provide a safer next-step.

Related:
- [EXEC-025] Exec Host, Security, and Approvals
- [EXEC-070] Reporting
- `systems/constraints/rules/exec-tool/00-INDEX.md`
