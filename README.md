# Clawguard

**Clawguard is the constraints, routing, and governance layer of the OpenClaw ecosystem — the guardrail system that keeps action bounded, reviewable, and legible.**

<p align="center">
  <img src="https://raw.githubusercontent.com/LKCY23/clawguard/main/assets/clawguard-banner.jpeg" alt="Clawguard cover" width="100%">
</p>

---

## Why this exists

As the ecosystem grew, constraints stopped being just scattered notes and became a real system.

Clawguard exists to define:

- what actions are allowed or blocked
- how tasks route into the right rule domains
- how runtime boundaries, safety checks, and governance stay explicit

It is the guard layer of the ecosystem.

---

## What it contains

- `CONSTRAINTS.md` — conceptual model and taxonomy
- `CONSTRAINTS_SYSTEM.md` — governance, conflicts, lifecycle
- `CONSTRAINTS_RUNTIME.md` — runtime procedure and gates
- `registry/` — lightweight routing entries
- `constraints/` — pack entrypoints
- `rules/` — active, draft, and deprecated rule packs

---

## High-level role

Clawguard is responsible for:

- constraints and policy boundaries
- routing high-risk or state-changing work through the right packs
- runtime discipline, receipts, and safety checks
- keeping the ecosystem governable as it expands

---

## Architecture

Clawguard is layered rather than flat.

Its main layers are:

- `CONSTRAINTS.md` — the conceptual model and taxonomy
- `CONSTRAINTS_SYSTEM.md` — governance, conflicts, lifecycle, and evolution rules
- `CONSTRAINTS_RUNTIME.md` — runtime procedure, gates, and receipt discipline
- `registry/` — intent routing and minimal-read selection
- `constraints/` — pack entrypoints and runtime-facing loading surfaces
- `rules/` — authoritative active, draft, and deprecated rules

This layering keeps concept, governance, runtime, routing, and domain rules from collapsing into one document.

---

## How it works

At a high level, Clawguard works like this:

1. a task arrives
2. the task routes through `registry/` into the relevant domain
3. the matching pack entrypoint under `constraints/` loads the minimal rule surface
4. the relevant rule pack under `rules/` provides the authoritative behavior for that domain
5. runtime gates, receipts, and execution discipline are applied through `CONSTRAINTS_RUNTIME.md`
6. the task executes under explicit governance rather than ad-hoc judgment

This is what makes Clawguard a real governance system rather than just a pile of rules files.

---

## Relationship to the wider ecosystem

- **Clawframe** provides the umbrella structure
- **Clawforge** handles learning and distillation
- **Clawguard** handles constraints, rules, and governance
- **Clawworks** handles development coordination

Clawguard is the layer that says not just what the ecosystem can do, but how it must do it.
