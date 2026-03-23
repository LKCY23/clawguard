# systems/constraints/rules/exec-tool/active/EXEC-040-screenshots.md

Title: Screenshots / Screen Capture

ID: EXEC-040
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-02-25

When:
- Any time the agent needs to capture a screenshot or a single-frame view of the current screen.

Rule:
- MUST clarify the capture target: gateway host screen, node device screen, or a browser page.
- MUST NOT assume the gateway host has an interactive GUI session or screen-recording permissions.
- SHOULD prefer node-based capture when the user intends to capture the user’s desktop/session and a node is available.
- For gateway host screenshots on macOS:
  - MUST prefer `/usr/sbin/screencapture` (absolute path) when available.
  - MUST NOT rely on PATH containing `/usr/sbin` for `screencapture`.
  - SHOULD use `-x` to suppress sounds.
- MAY fall back to `/opt/homebrew/bin/ffmpeg` + `-f avfoundation` when `screencapture` is unavailable or blocked.
- SHOULD write screenshot outputs to workspace-relative paths (e.g., `./deploy-screen.png`).

Rationale:
- The right capture method depends on whether the target is the gateway host, a user device (node), or a web page.
  `screencapture` is reliable on macOS when called by absolute path; `ffmpeg` provides a fallback.

Examples:
- Preferred (macOS gateway host):
  - `/usr/sbin/screencapture -x ./deploy-screen.png`
- Fallback device discovery (avfoundation):
  - `/opt/homebrew/bin/ffmpeg -f avfoundation -list_devices true -i ""`
- Fallback single-frame capture (device index varies; confirm via list_devices):
  - `/opt/homebrew/bin/ffmpeg -f avfoundation -i "<screen-index>" -frames:v 1 -y ./deploy-screen.png`

Failure Modes:
- No GUI / no permission on gateway host -> use node capture (connected=true) or use browser screenshot if the target is a web page.
- `command not found: screencapture` -> use `/usr/sbin/screencapture` or probe under login shell.
- avfoundation index differs across machines -> always run `-list_devices true` before selecting the index.

Related:
- [EXEC-010] Wrapper Selection
- [EXEC-020] PATH & Binary Resolution
- [EXEC-030] Where To Run
- `systems/constraints/rules/exec-tool/00-INDEX.md`
