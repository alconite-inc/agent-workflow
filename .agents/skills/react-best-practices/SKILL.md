---
name: react-best-practices
description: Implement or review React components, Hooks, state ownership, and interaction tests. Applies to React UI; use Next.js guidance for App Router server boundaries and do not apply React patterns to Qwik.
---

# React best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, component conventions, and checks. Apply this technology guidance
with the assigned generic role; it neither launches agents nor expands write ownership.
Identify whether this is a client-only Vite app or part of a server framework.

## Rendering and state

- Keep render pure and treat props and state as immutable snapshots. Put user
  actions in handlers and external synchronization in effects. Preserve Hook
  ordering and the project's Hook lint rules instead of suppressing them.
  See [component and Hook purity](https://react.dev/reference/rules/components-and-hooks-must-be-pure).
- Derive values during render when they can be computed from current props and
  state. Avoid effects that copy props, duplicate server-query data, or initiate
  work that belongs to one user event. When an effect is needed, provide cleanup
  and complete dependencies. See [when effects are needed](https://react.dev/learn/you-might-not-need-an-effect).
- Give each state one owner. Use the starter's existing router, data-fetching,
  and form conventions; preserve cancellation and invalidation boundaries when
  those facilities exist. Do not add a second cache to solve a local UI issue.
- Use stable identity for list keys and preserve local state intentionally when
  switching views. Show distinct pending, empty, failure, and success states for
  asynchronous interactions.
- Preserve the existing accessible primitives, labels, focus behavior, and
  semantic controls. Client validation improves feedback; the API still owns
  validation and authorization. In Vite apps, keep credentials out of browser
  configuration and preserve the development/production API proxy agreement.

## Verification focus

Use the existing interaction tests with user-visible queries and realistic user
events. Verify validation, pending states, error recovery, and keyboard behavior
for changed flows. Check effect cleanup when subscriptions or asynchronous work
change. Follow the root's build and type checks; component tests alone do not
verify a proxy or a Next.js server boundary.
