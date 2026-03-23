Title: Network and Port Exposure

ID: DOCKER-040
Status: active
Pack: docker-control
Owner: user
Last Updated: 2026-03-07

When:
- Before publishing container ports.
- Before attaching containers to networks that affect reachability.
- Before changing network topology or exposure scope for a service.

Rule:
- MUST minimize network exposure by default.
- MUST publish only the ports required for the stated service purpose.
- MUST prefer the smallest exposure scope possible.
- MUST avoid broad public exposure unless the user explicitly requests it and understands the consequence.
- MUST call out intended exposure in the receipt for non-trivial services.
- SHOULD prefer isolated Docker networks for multi-service applications when feasible.
- SHOULD verify the final published ports after deployment.
- MUST treat changes that widen exposure scope as high-risk.

Rationale:
- Many Docker incidents are exposure problems, not application problems.
- The safest port is the one that is not published.
- Small exposure scope reduces accidental reachability and simplifies reasoning about access.

Examples:
- Safer:
  - publish only the one HTTP port needed by SearXNG
- Riskier:
  - publishing many ports by default without a clear reason
- Receipt wording:
  - `Ports: 127.0.0.1:8888->8080/tcp`

Failure Modes:
- The service works but is exposed on more interfaces than intended -> reduce the binding scope.
- The agent publishes extra ports “just in case” -> remove them unless explicitly needed.
- Network design is unclear -> stop and clarify intended reachability.

Related:
- `systems/constraints/rules/docker-control/00-INDEX.md`
- `systems/constraints/rules/docker-control/active/DOCKER-010-connection-and-target-verification.md`
- `systems/constraints/rules/docker-control/active/DOCKER-070-receipts-and-verification.md`
