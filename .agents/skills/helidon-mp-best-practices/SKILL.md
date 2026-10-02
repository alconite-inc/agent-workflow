---
name: helidon-mp-best-practices
description: Implement or review Helidon MicroProfile Jakarta REST resources, CDI wiring, configuration, health, and container tests. Use for Helidon MP rather than programmatic Helidon SE composition.
---

# Helidon MP best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, Maven setup, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Use the Jakarta and MicroProfile APIs supplied by the project's Helidon BOM.

## Managed application boundaries

- Preserve Jakarta REST resources and CDI-managed collaborators. Keep resource
  methods focused on request mapping and validation. Helidon MP discovers its
  server through CDI; do not replace that lifecycle with an SE server builder
  as an incidental refactor. See [MP server lifecycle](https://helidon.io/docs/v4/mp/server).
- Choose bean scopes intentionally. Avoid placing mutable request-specific
  identity or payload state in application-scoped collaborators. Preserve bean
  discovery and qualifiers so tests exercise the same wiring as production.
- Keep configuration externalized through the existing MicroProfile model.
  Validate required values at an appropriate startup boundary, and avoid
  returning configuration secrets in diagnostics or health details.
- Keep Jakarta REST paths, media types, exception mapping, and declared OpenAPI
  synchronized. Preserve health endpoints and distinguish service readiness
  from the process merely being alive when adding dependency checks.
- Keep API and implementation dependencies compatible with the BOM. Mixing
  arbitrary Jakarta/MicroProfile versions can compile while breaking provider
  discovery or runtime linkage; treat such upgrades as a coordinated change.

## Verification focus

Use plain unit tests for application decisions and the existing Helidon MP test
container for CDI, configuration, resources, and health. Reused containers or
test instances can retain state: isolate configuration and reset owned mutable
fixtures. Verify HTTP status and media behavior through the provided client.
See [MP container testing](https://helidon.io/docs/v4/mp/testing).
Run the root's Maven and contract checks; a mocked resource test does not prove
CDI discovery or application startup.
