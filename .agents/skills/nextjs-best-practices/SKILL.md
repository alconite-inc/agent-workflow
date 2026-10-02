---
name: nextjs-best-practices
description: Develop or review Next.js App Router routes, Server and Client Components, data access, and standalone deployment. Use for Next.js behavior rather than a generic React or Vite application.
---

# Next.js best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, deployment shape, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Use the installed Next.js version's routing, request, and caching APIs.

## Server and browser boundaries

- Keep App Router components on the server unless interactivity or browser APIs
  require a client boundary. Place `use client` at the smallest useful boundary;
  its imports enter the client module graph. Send only deliberately public,
  serializable props across that boundary. See [Server and Client Components](https://nextjs.org/docs/app/getting-started/server-and-client-components).
- Keep credentials and privileged data access in server-only modules. Route
  handlers and server actions must validate input and authorize each operation;
  a hidden button or an unexported UI path is not an authorization boundary.
- Decide whether data is public/cacheable or user-specific before choosing
  cache and revalidation settings. Check the installed version's defaults;
  do not let a shared cache serve one user's private response to another.
- Public environment values are embedded at build time. Runtime container
  environment changes cannot rewrite an already built `NEXT_PUBLIC_*` bundle.
  Keep API origins distinct for browser and server access when their network
  reachability differs. See [self-hosting and environment behavior](https://nextjs.org/docs/app/guides/self-hosting).
- Preserve standalone output, static assets, and the existing independent API
  deployment when present. Adding a server action does not transfer ownership
  of backend business rules out of the API service.

## Verification focus

Use existing unit/component tests for local logic and interactions. Exercise
request authorization, rendering, and cache isolation through a running app when
those behaviors change. Build the production output and verify changed routes in
that mode; a browser-like unit environment cannot prove Server Component or
standalone-container behavior. Follow the root's deployment and contract checks.
