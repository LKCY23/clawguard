Title: Image Trust and Versioning

ID: DOCKER-020
Status: active
Pack: docker-control
Owner: user
Last Updated: 2026-03-07

When:
- Before pulling, building, tagging, pushing, recreating, or deploying a container image.

Rule:
- MUST prefer trusted image sources.
- MUST prefer official images or clearly attributable publishers when available.
- MUST avoid casually deploying unknown third-party images without calling out the source.
- MUST prefer stable tags or digests when reproducibility matters.
- MUST NOT rely on floating `latest` tags for long-lived or production-like services unless the user explicitly prefers convenience over reproducibility.
- MUST state the chosen image and tag in receipts for non-trivial deployments.
- SHOULD verify that the pulled or built image matches the intended repository/tag before deployment.

Rationale:
- Docker images are executable supply-chain inputs.
- Trust and reproducibility matter more for long-lived or remotely managed services.
- Stable tags reduce accidental drift when recreating workloads later.

Examples:
- Preferred:
  - `searxng/searxng:2026.3.0`
- Less reproducible:
  - `searxng/searxng:latest`
- Receipt wording:
  - `Image: searxng/searxng:2026.3.0`

Failure Modes:
- The agent deploys an image with unclear provenance -> stop and identify the publisher/source.
- A service is recreated with a floating tag and changes unexpectedly -> switch to a stable tag or digest.
- The user asks for speed over stability -> note the drift risk explicitly.

Related:
- `systems/constraints/rules/docker-control/00-INDEX.md`
- `systems/constraints/rules/docker-control/active/DOCKER-060-lifecycle-and-cleanup.md`
