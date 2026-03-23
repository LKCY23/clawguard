Title: Gateway Restart Handshake

ID: MAINT-020
Status: active
Pack: maintenance
Owner: user
Last Updated: 2026-03-04

When:
- Any time the agent is about to restart the OpenClaw gateway (e.g., `openclaw gateway restart`).

Rule:
- MUST use a 2-step restart flow to ensure the pre-restart notice is actually delivered:
  - Step 1 (notice-only): send a pre-restart notice and do NOT restart yet
  - Step 2 (restart): only restart after the user replies with an explicit confirmation keyword
    (default keyword: `go`)
- The pre-restart notice MUST include:
  - that a gateway restart is about to happen
  - expected duration (e.g., 5-15 seconds)
  - how the user can verify liveness (`openclaw gateway status`)
- The post-restart confirmation MUST be provided in the next user-visible reply, including:
  - restart complete
  - expected boot notification channels (Feishu/Telegram boot-md) when configured

Rationale:
- Restarts can interrupt the user's visibility and feel like the agent went silent.

Examples:
- Pre: "Preparing to restart the gateway (ETA 5-15s). You can run: `openclaw gateway status`."
- Post: "Restart complete. You should receive the boot-md online notification."

Failure Modes:
- Restart performed without notice -> treat as a bug; add the handshake and avoid repeating.

Related:
- [MAINT-010] OpenClaw Update Checks
- `workspace/BOOT.md`
