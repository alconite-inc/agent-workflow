---
name: axum-best-practices
description: Implement or review Axum routes, extractors, state, middleware, and HTTP tests. Applies to Rust HTTP adapters; use Rust guidance for language and library design.
---

# Axum best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, API contract, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Read APIs for the installed Axum and Tower versions before copying examples.

## Request and runtime boundaries

- Keep handlers as adapters from validated request data to application behavior.
  Prefer typed `State` for application dependencies; keep request identity in
  request-scoped data and validate authorization on the server.
- Extract request parts before the body-consuming extractor. A body stream can
  be consumed only once; JSON parsing proves deserialization, not business
  validity. Review rejection status, content type, and body limits together.
  See [extractor ordering and rejections](https://docs.rs/axum/latest/axum/extract/index.html).
- Map application errors into intentional HTTP responses and keep internal
  diagnostics out of response bodies. Preserve the public status/header/schema
  contract when changing error handling or middleware placement.
- Trace which routes a layer covers, including nested routers, fallbacks, and
  preflight requests. Preserve the starter's same-origin proxy or explicit CORS
  allowlist; CORS is not a substitute for account authorization.
- Keep blocking I/O and long CPU work off Tokio executor threads. If using
  `spawn_blocking`, bound concurrency and account for shutdown: a blocking task
  that has started cannot be aborted like an async task.
  See [blocking-task behavior](https://docs.rs/tokio/latest/tokio/task/fn.spawn_blocking.html).

## Verification focus

Exercise the composed router through the existing Tower test harness so tests
cover extractors and middleware, not only handler helpers. Include invalid JSON,
invalid domain values, missing authorization when applicable, and intended HTTP
statuses. Verify startup/shutdown separately when changed. Follow the root's API
contract checks and deployment boundary; an in-process route test does not prove
proxy, socket, TLS, or container behavior. See [Axum composition](https://docs.rs/axum/latest/axum/).
