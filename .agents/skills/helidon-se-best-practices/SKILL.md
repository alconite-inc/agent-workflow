---
name: helidon-se-best-practices
description: Implement or review Helidon SE programmatic WebServer routing, configuration, and service tests. Use for the SE stack; MicroProfile/CDI resource conventions belong to Helidon MP.
---

# Helidon SE best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, Maven setup, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Use the installed Helidon generation's APIs rather than older reactive examples.

## Programmatic server composition

- Preserve explicit server construction, configuration, and route registration.
  Keep HTTP translation in services and application decisions behind them;
  adding CDI or MicroProfile annotations changes the chosen composition model.
- The current SE WebServer uses virtual threads. Complete request handling on
  its request thread; do not hand off `res.send()` to an arbitrary async callback.
  Virtual threads do not remove downstream connection-pool, timeout, or
  concurrency limits. See [WebServer request handling](https://helidon.io/docs/v4/se/webserver).
- Make route continuation versus terminal response behavior deliberate. Review
  route order, missing routes, media conversion, and exception mapping when
  adding handlers; avoid sending a second response after completion.
- Keep media support and its registered dependencies aligned with request and
  response types. Preserve configured body limits and intentional status codes,
  and synchronize public HTTP changes with the project's API contract.
- Externalize ports and environment settings through the existing configuration
  model. Close owned clients and servers at shutdown; do not leave test listeners
  running or introduce fixed ports into concurrent tests.

## Verification focus

Reuse the existing Helidon SE test harness. Direct routing tests cover handler
composition; server tests additionally exercise the network boundary. Choose the
level needed for the changed behavior and check malformed inputs and failure
responses, not only the success body. See [SE testing](https://helidon.io/docs/v4/se/testing).
Run the root's Maven verification and API checks. A routing test does not establish
container startup, TLS, or production configuration behavior.
