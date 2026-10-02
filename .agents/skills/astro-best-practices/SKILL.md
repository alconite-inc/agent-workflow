---
name: astro-best-practices
description: Develop or review Astro pages, static output, browser scripts, and selective hydration. Use for the Astro website and its packaging boundary; runtime backend APIs remain owned by the paired service.
---

# Astro best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, package scripts, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Inspect the configured output mode before adding runtime features.

## Static output and interaction

- Preserve the starter's static output and backend packaging contract. Component
  frontmatter and prerendered routes run at build time, not once per visitor.
  Runtime request handlers, sessions, or server islands require a compatible
  adapter and an intentional deployment change. See [rendering modes](https://docs.astro.build/en/guides/on-demand-rendering/).
- Keep runtime API behavior in the paired service. Fetching private or changing
  data during a static build can freeze it into output; do not treat build-time
  data access as request-time authorization.
- Prefer Astro markup and focused browser scripts for simple interactions.
  When a framework integration is justified, hydrate only the required islands
  with an appropriate `client:*` directive and serializable public props.
  See [framework components and hydration](https://docs.astro.build/en/guides/framework-components/).
- Treat `PUBLIC_*` values as browser-visible. Vite environment substitutions are
  made during the build; changing the Spring process environment does not alter
  the already packaged static site. See [environment variables](https://docs.astro.build/en/guides/environment-variables/).
- Preserve the build dependency that packages fresh website assets into the
  backend artifact. Check asset URLs and route fallback behavior under the
  deployed base path; a local development server can hide packaging mistakes.

## Verification focus

Use the checks the root and package scripts actually define. Inspect built pages,
asset references, and responsive/keyboard behavior for changed views. When the
packaging boundary changes, exercise the packaged backend artifact and its API;
an Astro build alone does not prove that Java resources contain current assets.
