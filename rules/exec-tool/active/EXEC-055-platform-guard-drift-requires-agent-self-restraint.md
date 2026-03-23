Title: Platform Guard Drift Requires Agent Self-Restraint

ID: EXEC-055
Status: active
Pack: exec-tool
Owner: user
Last Updated: 2026-03-10

When:
- The agent is about to run `functions.exec`.
- The effective runtime guard behavior (`ask`, approvals, sandbox host routing, or allowlist behavior) is known to be unreliable, inconsistent, or materially different from expected documentation semantics.
- The current configuration allows broad execution power (for example `security: "full"` and/or `ask: "off"`), reducing platform-enforced friction.

Rule:
- The agent MUST treat platform guard behavior as a helpful control when it works, but MUST NOT rely on it as the sole safety boundary.
- When runtime guard behavior is inconsistent, the agent MUST fall back to rule-based self-restraint before executing commands.
- The agent MUST continue to require explicit user confirmation before running high-risk or state-changing commands, even if the platform would technically allow them without additional prompts.
- At minimum, the agent MUST stop and ask before:
  - destructive deletions
  - overwriting or bulk-editing user files outside the requested scope
  - daemon/service lifecycle changes
  - global configuration edits
  - package installs/removals that change the host environment
  - network/security posture changes
  - outbound/public actions triggered through shell commands
- The agent MUST treat “platform allowed it” as insufficient justification for high-risk execution.
- The agent MUST treat “platform blocked it unexpectedly” as a diagnostic signal, not as permission to weaken unrelated constraints.
- When using `security: "full"` for a command that would otherwise be blocked under `allowlist`, the agent SHOULD ensure the command is narrow, relevant, and proportionate to the task.
- The agent SHOULD prefer the least-powerful execution posture that still completes the task reliably.
- The agent SHOULD explicitly note when a command required broader execution policy because the narrower platform guard path was unreliable or over-restrictive.
- This rule complements, and does not replace, any stricter preflight, receipt, or confirmation requirement defined elsewhere in this pack or in `systems/constraints/CONSTRAINTS_RUNTIME.md`.

Rationale:
- Platform guard behavior can drift across versions, bridges, providers, or session surfaces.
- If the agent assumes the runtime guard is always reliable, then broad execution settings such as `security: "full"` or `ask: "off"` create unsafe silent expansion of power.
- A durable safety model requires the agent’s own rules to remain authoritative when platform enforcement is weak, inconsistent, or surprising.

Examples:
- Good:
  - A governance lint command is blocked under `allowlist`; the agent uses `security: "full"` for that narrow verification command and reports the deviation.
- Good:
  - The platform no longer prompts for a risky command because `ask: "off"` is configured; the agent still pauses and asks the user before restarting a service.
- Bad:
  - The agent observes that approvals are not firing reliably and starts running destructive shell commands without confirmation because the platform no longer blocks them.
- Bad:
  - The agent broadens from a blocked `allowlist` command to unrelated high-power commands under `security: "full"` without separately evaluating risk.

Failure Modes:
- Broad runtime power silently replaces deliberate judgment.
- The agent mistakes runtime inconsistency for user authorization.
- Platform approval drift causes the agent to stop asking before high-risk actions.
- A narrow need for `security: "full"` expands into habitual unrestricted execution.

Related:
- `systems/constraints/rules/exec-tool/00-INDEX.md`
- `systems/constraints/rules/exec-tool/active/EXEC-025-exec-host-security-approvals.md`
- `systems/constraints/rules/exec-tool/active/EXEC-060-exec-safety.md`
- `systems/constraints/rules/exec-tool/active/EXEC-080-stateful-exec-actions-require-preflight-receipt.md`
- `systems/constraints/CONSTRAINTS_RUNTIME.md`
