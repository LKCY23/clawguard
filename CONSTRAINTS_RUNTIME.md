# CONSTRAINTS_RUNTIME.md

Operational playbook: how to apply the constraint system during normal agent work.

Authoritative for:
- The runtime procedure: route-first, minimal reads, and execution defaults.
- When a receipt is required and the exact receipt format.
- Backfilling receipts after the fact.

Not authoritative for:
- The constraint model vocabulary and pack taxonomy (see `systems/constraints/CONSTRAINTS.md`).
- Governance, conflicts, and rule lifecycle (see `systems/constraints/CONSTRAINTS_SYSTEM.md`).
- Domain behavior rules (those live under `systems/constraints/rules/<pack>/...``).

This file is intentionally short and execution-oriented.
Single source of truth:
- Routing + minimal reads: `systems/constraints/registry/*.yml`
- Constraint entrypoints: `systems/constraints/constraints/<pack>/00-INDEX.md`
- Domain rules: `systems/constraints/rules/<pack>/...``
- Governance + conflicts + lifecycle: `systems/constraints/CONSTRAINTS_SYSTEM.md`
- Constraint model (domain vs pack, taxonomy): `systems/constraints/CONSTRAINTS.md`

It complements:
- `systems/constraints/CONSTRAINTS_SYSTEM.md` (governance framework)
- `systems/constraints/registry/*.yml` (routing + minimal reads)
- `systems/constraints/constraints/<pack>/00-INDEX.md` + `systems/constraints/rules/<pack>/...`` (domain rules)

---

## 1) Always: Route First (Registry)

Before doing any non-trivial work, determine the primary domain(s) and match registry entries.

- Prefer `toolSignals` matches (imminent tool use) over free-text `triggers`.
- Domains can stack (example: `exec` + `repo-hygiene`).

Minimal behavior:
- If a registry entry matches, read exactly its `minimalReadMain`/`minimalReadShared` (and nothing extra).
- If no entry matches, proceed carefully and ask one clarifying question when risk is non-trivial.

---

## 2) When To Produce A Receipt (And When)

Receipts are primarily for audit / review. Default timing is **post-action**.

### 2.1 Post-action (Default)

A receipt is required when the task routes to one or more registry entries and you took any non-trivial action (anything beyond read-only observation).

"Non-trivial action" includes (non-exhaustive):
- writing/editing files (beyond temporary scratch output)
- running commands that change local or remote state (installs, services, network, git push, etc.)
- sending/editing/deleting messages externally
- browser/nodes actions beyond read-only observation

- If you matched one or more `systems/constraints/registry/*.yml` entries, you MUST produce a receipt after execution.
- If you did not match any registry entry, you SHOULD avoid executing non-trivial actions; first route (or ask one clarifying question), then proceed.

`riskLevel` guidance:
- `riskLevel: high` -> receipt is REQUIRED (post-action) for any non-trivial action.
- `riskLevel: medium` -> receipt is REQUIRED (post-action) for any non-read-only action.
- `riskLevel: low` -> receipt is OPTIONAL.

### 2.2 Pre-action (Gate)

Pre-action receipts/confirmations are required when either:
- a domain rule explicitly requires user confirmation before execution (e.g. confirmation tokens), or
- the action is very high-risk and user intent/authorization is unclear.

### 2.3 Pre-action Self-Check For Persistent State Changes

The following actions MUST be treated as non-trivial state-changing actions even when they are local, scoped, reversible, or performed during investigation:

- persistent configuration changes
- daemon/service lifecycle changes
- local state changes that alter subsequent runtime behavior
- durable tool/runtime configuration changes outside the workspace repo

Before executing any such action, the agent MUST perform a short pre-action self-check and MUST provide a receipt-style summary.

This requirement does NOT imply that additional user approval is always required.
Its purpose is:
- auditability
- operator review and later reconstruction
- improving execution quality and safety by forcing an explicit check of intent, scope, and verification before action

“Small”, “quick”, “just config”, “local only”, or “while investigating” do NOT exempt an action from this requirement if the action changes persistent state or later behavior.

Hard gate (services/config/hooks):

For any of the following actions, you MUST obtain an explicit user confirmation token in the same conversation before executing:
- restarting/stopping/starting the OpenClaw gateway service
- enabling/disabling hooks
- editing global config (`~/.openclaw/openclaw.json`)

Default token format (recommended): `CONFIRM-<ACTION>` (example: `CONFIRM-RESTART`).
If a token is not present, stop and ask for it (do not proceed).

Examples (non-exhaustive):
- Outbound messaging without explicit send authorization (see messaging pack, e.g. `MSG-010`).
- Any action requiring an explicit confirmation token (e.g. `REPO-070` for `git push`).
- Irreversible or high-blast-radius operations (deletion, restarts, external/public actions) when the request is ambiguous.

Conflicts are resolved per `systems/constraints/CONSTRAINTS_SYSTEM.md`.

---

## 3) Receipt Format (Short, Auditable)

Keep receipts terse. Use this exact structure:

- Intent: <one line>
- Registry: <matched entries, e.g. REGISTRY-EXEC, REGISTRY-REPO>
- Applied: <pack + RULE-IDs (required)>
- Plan: <steps you will take; include rollback/stop condition if applicable>
- Execute: <exact commands/tools invoked; include key targets/args when it matters>
- Verify: <1-3 objective checks (commands/observations) that would fail if the intent failed>
- Deviations: <"none" or explain why you deviated>

Notes:
- The `Applied` line MUST include RULE-IDs (e.g. `MSG-010`, `REPO-070`).
- `Intent` SHOULD be testable (avoid vague verbs like "handle"; prefer "push", "restart", "create").
- `Plan` SHOULD be short (1-4 steps) and mention a stop/rollback condition when risk is non-trivial.
- `Execute` MUST be specific enough to reproduce; if you omit long args/logs, cite a path and summarize.
- Unless a domain rule requires a pre-action gate, receipts SHOULD be produced post-action; use backfill when you forgot.
- `Verify` SHOULD be reproducible and reference concrete artifacts (commit id, file path, service status).
- In SHARED contexts, follow `outputPolicyShared` and redact per `redactionCategories`.
- Do not paste long logs; summarize and cite paths.

---

## 4) Execution Defaults

- Prefer the least-destructive action.
- Prefer deterministic commands (explicit paths/versions) when it matters.
- If a tool returns an explicit non-retryable failure ("Do NOT retry"), stop and switch to the prescribed recovery path.

---

## 5) After The Fact (Backfill)

If you already executed an action without a receipt:

- Immediately produce a backfilled receipt.
- Explicitly mark it as backfilled.
- Re-run verification steps to restore auditability.
