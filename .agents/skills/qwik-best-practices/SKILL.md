---
name: qwik-best-practices
description: Implement or review Qwik components, resumable handlers, Qwik City routes, and Node deployment. Use for Qwik applications; do not substitute React lifecycle or hydration patterns.
---

# Qwik best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, adapter configuration, and checks. Apply this technology guidance
with the assigned generic role; it neither launches agents nor expands write ownership.
Use documentation matching the installed Qwik major version.

## Resumability and request boundaries

- Use Qwik's `$` APIs for lazy callbacks and event handlers. Keep captured values
  serializable and small; avoid capturing a service client or large object graph
  in a handler. Check optimizer constraints instead of replacing QRLs with React
  callbacks. See [closure extraction rules](https://qwik.dev/docs/advanced/dollar/).
- Use signals and stores for reactive state. Keep browser-only work out of
  server execution; use lifecycle APIs only where their execution timing is
  needed, and clean up timers or subscriptions. Do not eagerly run client work
  simply to reproduce a React mount effect.
- Use route loaders for server-side route data and actions for mutations when
  those patterns are present. Returned loader data becomes available to the UI;
  select public fields explicitly and enforce authorization on the server.
  See [route loaders](https://qwik.dev/docs/route-loader/).
- Preserve the Qwik City Node adapter and both client/server build outputs.
  Keep server-only configuration separate from `PUBLIC_*` values. When paired
  with an API service, preserve that service's independent deployment and
  authorization boundary.
- Configure production `ORIGIN` for the public site origin. Preserve the
  framework's origin checks instead of disabling them to solve a proxy mismatch.
  See [Node deployment and origin checks](https://qwik.dev/docs/deployments/node/).

## Verification focus

Run the root's actual lint, type, test, format, and build checks. Verify SSR output
and resumed interaction in a browser when handlers change; pure helper tests do
not exercise serialization or chunk loading. Check form errors and production
origin/proxy behavior when route actions or deployment configuration change.
