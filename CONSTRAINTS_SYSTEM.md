# CONSTRAINTS_SYSTEM.md

A small, durable system for writing and evolving rules over time.

Authoritative for:
- Governance of the rules system: file layout, rule templates, and lifecycle.
- Conflict resolution and priority ordering when rules disagree.
- How new rules are proposed, reviewed, activated, and deprecated.

Not authoritative for:
- Runtime procedure (route-first, receipts, execution defaults) (see `systems/constraints/CONSTRAINTS_RUNTIME.md`).
- The constraint model vocabulary and pack taxonomy (see `systems/constraints/CONSTRAINTS.md`).
- Domain behavior rules content (those live under `systems/constraints/rules/<pack>/...``).

This file defines:
- how rules are organized
- how each rule is written
- how conflicts are resolved
- how new rules are proposed, reviewed, activated, and deprecated

The goal is long-term iterability: adding a new rule should be easy, predictable,
and should not bloat a single “mega doc”.

Single source of truth:
- Governance + conflicts + lifecycle: `systems/constraints/CONSTRAINTS_SYSTEM.md`
- Constraint model (domain vs pack, taxonomy): `systems/constraints/CONSTRAINTS.md`
- Runtime procedure + receipts: `systems/constraints/CONSTRAINTS_RUNTIME.md`
- Routing + minimal reads: `systems/constraints/registry/*.yml`
- Constraint entrypoints: `systems/constraints/constraints/<pack>/00-INDEX.md`
- Domain rules: `systems/constraints/rules/<pack>/...``

---

## 1) Scope

This rules system applies to all rule domains, for example:
- execution / shell / tooling (`exec`, `process`, `browser`, `nodes`)
- messaging and tone
- safety and privacy
- code editing conventions
- workspace hygiene and documentation
- memory policy

This system does NOT define the domain rules themselves. It defines the framework
for them.

---

## 2) File Layout (Thin Index + Rule Directory)

## Related Governance

This charter governs the rules pack system under `systems/constraints/rules/`.

On-demand file loading and MAIN/SHARED read/write boundaries are governed by:
- `AGENTS.md` -> "On-Demand Files Registry" section

Do not duplicate the on-demand registry schema in this document.

Rules SHOULD be organized as:
- one thin index / charter file per domain (stable, rarely changes)
- many small rule files (easy to add/iterate)

Recommended workspace layout:

- `systems/constraints/CONSTRAINTS_SYSTEM.md` (this file; the global framework)
- `systems/constraints/rules/` (all rule packs)
  - `systems/constraints/rules/README.md` (what packs exist + quick links)
  - `systems/constraints/rules/<pack>/` (one pack per domain)
    - `00-INDEX.md` (thin pack index: terms, decision order, links)
    - `templates/`
      - `RULE_TEMPLATE.md`
    - `active/`
      - `10-some-rule.md`
      - `20-another-rule.md`
    - `draft/`
      - `10-proposed-rule.md`
    - `deprecated/`
      - `10-old-rule.md`
    - `CHANGELOG.md` (optional; git history is acceptable alternative)

Notes:
- A “pack” is a domain, not a project. Example packs: `exec-tool`, `messaging`,
  `safety`, `frontend`, `repo-hygiene`.
- Packs MUST stay small at the index layer; details MUST live in per-rule files.

---

## 2.1) Pack-local Runtime Settings

A constraints pack MAY define a small number of pack-local runtime settings files for tunable strategy parameters.

Purpose:
- keep normative principles in rule files
- keep tunable execution parameters out of rule text
- allow behavior tuning without rewriting rule documents

Requirements:
- Rule files remain authoritative for principles, obligations, and boundaries.
- Pack-local runtime settings files are for parameters and switches, not for replacing rule intent.
- Pack-local runtime settings MUST be loaded through the pack's constraint entrypoint (`systems/constraints/constraints/<pack>/00-INDEX.md`), not treated as independent routing targets.
- Registry routing SHOULD continue to target the constraint entrypoint, and SHOULD NOT be expanded to route directly to pack-local settings files.
- Pack-local runtime settings SHOULD stay small, explicit, and pack-scoped.

Examples:
- `systems/constraints/constraints/web-search/settings.json` for search strategy parameters

## 3) Normative Language (BCP 14: RFC 2119 / RFC 8174)

Interpretation statement:
- The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
  "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this
  document are to be interpreted as described in RFC 2119.
- Per RFC 8174 (together with RFC 2119 commonly referred to as BCP 14), these
  keywords are normative ONLY when they appear in ALL CAPS. Rule packs MUST use
  ALL CAPS for normative keywords.

Clarification:
- Normative requirements MAY be expressed without using these keywords; the
  keywords exist to make requirements unambiguous and consistent, not to limit
  what “counts” as a rule.

When these keywords are used, they MUST be used consistently:

- `MUST`: required. violating it is a bug unless explicitly overridden.
- `MUST NOT`: prohibited. doing it is a bug unless explicitly overridden.
- `SHOULD`: default best practice. deviations are allowed with a reason.
- `SHOULD NOT`: discouraged. do it only with a reason and awareness of tradeoffs.
- `MAY`: optional. use when multiple choices are acceptable.

If a rule cannot be expressed with MUST/SHOULD/MAY, it is likely a guideline and
SHOULD be rewritten or moved to examples.

---

## 4) Rule Lifecycle (Draft -> Active -> Deprecated)

### 4.1 Draft

A draft rule is proposed and written, but not yet enforced by default.
Draft rules live under:
- `systems/constraints/rules/<pack>/draft/`

Draft semantics:
- Draft rules MUST NOT be treated as default behavior.
- Draft rules MAY be applied only when:
  - the user explicitly asks to trial them, or
  - the user is currently reviewing/approving the draft, or
  - the draft is necessary to resolve an ambiguity and the user prefers it.

### 4.2 Review

A rule becomes active only after the user reviews it.
The review output MUST be a complete markdown file (not partial snippets), so it
can be copied into place without guesswork.

### 4.3 Active

After approval, a rule moves to:
- `systems/constraints/rules/<pack>/active/`

Active semantics:
- Active rules are what the agent MUST follow by default.

### 4.4 Deprecated

When a rule is replaced, it moves to:
- `systems/constraints/rules/<pack>/deprecated/`
and MUST include:
- what replaced it
- why it was deprecated
- since when

ID stability:
- Rule IDs MUST be stable over time.
- Deprecated rule IDs MUST NOT be reused.

---

## 5) Conflict Resolution (When Rules Disagree)

When two rules conflict, resolve using this priority order:

1) Safety & Privacy
2) Correctness (producing the right result)
3) Security posture (least privilege, least exposure)
4) Reproducibility (determinism, explicit versions/paths)
5) Maintainability (clarity, small diffs, conventions)
6) Convenience & Speed

If still ambiguous:
- choose the safer option
- record the ambiguity as a new draft rule or an addendum to the pack index

A rule MAY explicitly override a lower-priority rule, but MUST state:
- what it overrides
- why the override is safe/necessary
- its scope boundary (when it applies)

---

## 6) Pack Index Requirements (`00-INDEX.md`)

Each pack MUST have an index (`systems/constraints/rules/<pack>/00-INDEX.md`) that contains only:

- Purpose: what this pack governs
- Non-goals / Out of scope: what this pack explicitly does NOT govern
- Entry Decision Tree: a short (1-5 line) navigation-first decision tree
- Terms: definitions for key terms used across rules
- Decision Order: the order to decide things in this domain
- Quick Priority Notes: pack-specific tie breakers if needed
- Links: list of active rules with one-line descriptions

Decision Order precedence:
- When rules within the same pack appear to conflict, the pack’s `Decision Order`
  MUST be applied first.
- If conflict remains after applying the pack’s `Decision Order`, use the global
  conflict resolution order in section 5.

Link format:
- Index links SHOULD use a stable rule-ID reference format, for example:
  - `- [EXEC-012] Title — one line description`

The index MUST NOT become a full rule dump. If it grows too large, split content
into new per-rule files and link them.

---

## 7) Standard Rule File Template (Required Fields)

Every rule file MUST follow this template (sections required unless noted):

1) `Title`
2) `ID`: `<PACK>-<NNN>` (stable identifier; filename MAY change)
3) `Status`: `draft` | `active` | `deprecated`
4) `Pack`: `<pack-name>`
5) `Owner`: `user`
6) `Last Updated`: `YYYY-MM-DD`
7) `When` (scope / trigger conditions)
8) `Rule` (normative statements using MUST/SHOULD/MAY)
9) `Rationale` (why)
10) `Examples` (copy-pasteable examples)
11) `Failure Modes` (common mistakes + what to do)
12) `Related` (links to other rules)

Optional:
- `Overrides` (only if it overrides something)
- `References` (links to docs)

---

## 8) Adding a New Rule (Procedure)

When the user asks to “add a rule”:

1) Identify the correct pack (or propose a new pack if needed).
2) Create a draft rule using the standard template.
3) Keep the rule small:
   - prefer multiple small rules over one large one
   - move examples and edge cases into separate rules if they expand too far
4) Present the full markdown for review.
5) After approval, mark it active and move it into `active/`.
6) Update the pack index to link it.

---

## 9) Editing Existing Rules (Procedure)

When changing a rule:

- Prefer additive changes:
  - small edits, clear rationale
- If the change is major:
  - create a new rule file
  - deprecate the old one with a pointer to the new rule

---

## 10) Splitting Thresholds (Prevent Mega Rules)

A rule SHOULD be split into multiple rules when:
- the rule file exceeds ~120 lines, OR
- the `Examples` section exceeds 6 examples, OR
- the rule begins mixing multiple decision points that can stand alone.

When splitting:
- Keep the original rule’s intent stable.
- Move sub-decisions into new rules and add `Related` links both ways.
- Update the pack index to reference the new rules.

---

## 11) Minimal Changelog Policy

Changelog MAY be handled in one of two ways:
- Git history only (acceptable for most cases)
- `systems/constraints/rules/<pack>/CHANGELOG.md` with short entries (date + what changed + why)

Choose one per pack and stick to it.

---

## 12) Design Goals (What “Good” Looks Like)

A good rules system:
- makes it trivial to add one new rule without rewriting old docs
- keeps indexes thin and stable
- makes conflicts predictable
- separates “what to do” from “why” and “how”
- is searchable (small files, clear names, numbered prefixes)

## 12.1 Receipt Intent

Receipt and self-check requirements exist to improve:
- auditability
- post-incident reconstruction
- operator review
- execution quality and safety

They are not primarily approval theater.
A rule MAY require approval, but a self-check / receipt requirement MAY also apply when approval is not required.

---

## 13) Naming Conventions

- Rule filenames SHOULD start with a number for stable ordering:
  - `10-...md`, `20-...md`, `90-...md`
- Use kebab-case for filenames.
- Prefer ASCII only.

---

End.
