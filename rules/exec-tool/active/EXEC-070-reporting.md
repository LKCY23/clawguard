# systems/constraints/rules/exec-tool/active/EXEC-070-reporting.md

Title: Reporting Expectations

ID: EXEC-070
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-02-25

When:
- After any exec/process action that produces an artifact, changes state, or fails.

Rule:
- MUST include an "Applied rules" line listing the exec-tool rule IDs followed for this action (e.g., EXEC-060, EXEC-050).
- On success:
  - SHOULD provide a concise confirmation and the artifact location (path), OR send the artifact via `functions.message`.
- On failure:
  - MUST include the raw error line(s) and the exact command used (redacted if sensitive).
  - MUST propose the next best retry strategy (wrapper vs absolute path vs different target such as node).
- SHOULD avoid dumping large logs into chat; summarize and point to the relevant file/path.

Rationale:
- Clear reporting makes debugging faster and prevents repeated “what did you run?” back-and-forth.

Examples:
- Failure template:
  - Command: `<cmd>`
  - Error: `<first relevant error lines>`
  - Next: retry with `<strategy>`

Failure Modes:
- Missing context in report -> user cannot reproduce; add exact command + environment notes.

Related:
- [EXEC-020] PATH & Binary Resolution
- [EXEC-050] Long-Running & PTY
- [EXEC-060] Exec Safety
- `systems/constraints/rules/exec-tool/00-INDEX.md`
