---
name: spring-boot-best-practices
description: Develop or review Spring Boot configuration, dependency injection, service transactions, and MVC adapters. Applies to Spring services and workers; use Kafka guidance for message delivery semantics.
---

# Spring Boot best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, build wrappers, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Use the reference documentation for the project's Spring Boot major version.

## Framework boundaries

- Check the installed starters before choosing a web model. These HTTP starters
  use Spring MVC and servlet semantics. Adding a reactive client does not make
  the application reactive; adopting WebFlux requires a deliberate architecture
  change. A worker does not need an HTTP API solely because it uses Boot.
  See [MVC and reactive stack selection](https://docs.spring.io/spring-boot/reference/web/reactive.html).
- Use constructor injection for required collaborators and typed, validated
  configuration for runtime settings. Keep transport validation and response
  mapping in controllers; place use-case behavior behind that boundary.
- Preserve declared module dependency direction. In a modular starter, keep
  domain types independent of Spring and infrastructure implementations.
- Put transaction boundaries around complete business operations when persistence
  exists. Default proxy-based `@Transactional` does not intercept self-invocation;
  inspect rollback rules for checked exceptions and test a real rollback.
  See [transaction proxy semantics](https://docs.spring.io/spring-framework/reference/data-access/transaction/declarative/annotations.html).
- Keep error bodies deliberate and synchronized with the API contract. Avoid
  exposing exception internals or expanding Actuator exposure with a wildcard.

## Verification focus

Use plain tests for domain decisions, the existing MVC slice for request binding
and validation, and a full application test when wiring or configuration changes.
A real-server test runs requests on different threads from the test; a test
transaction does not automatically roll back server-side work. Isolate fixtures
and verify cleanup. See [Spring Boot testing](https://docs.spring.io/spring-boot/reference/testing/spring-boot-applications.html).
Run the generated root's checks; do not copy test imports from a different Boot major.
