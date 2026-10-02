---
name: typescript-best-practices
description: Design or review TypeScript types, data boundaries, and module contracts. Use for typed application code and API clients across frontend frameworks, with the relevant framework skill for rendering behavior.
---

# TypeScript best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, package scripts, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Keep the existing compiler strictness and runtime module model.

## Types at runtime boundaries

- Treat external JSON, stored data, environment values, and caught failures as
  untrusted. Parse or narrow them before use, reusing the project's validation
  library when present. A type assertion changes compiler assumptions and adds
  no runtime validation. See [type assertions](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#type-assertions).
- Model mutually exclusive outcomes with discriminated unions where that makes
  invalid combinations unrepresentable. Narrow by a stable discriminator and
  check exhaustiveness when every variant requires handling.
  See [narrowing and exhaustiveness](https://www.typescriptlang.org/docs/handbook/2/narrowing.html).
- Distinguish absent, null, empty, and zero when the domain does. Avoid truthiness
  checks that discard valid values, unchecked non-null assertions, or `any`
  used to erase an unresolved contract mismatch.
- Keep shared DTOs independent of browser-only and server-only modules. A
  type-only import does not justify importing runtime secrets into client code;
  check the framework's bundle and environment rules at each boundary.
- Preserve the installed bundler's module resolution and import conventions.
  Do not change compiler options or dependency versions merely to make an
  unrelated example type-check.

## Verification focus

Run the root's actual type-check command in addition to runtime tests and the
production build where required. Fast transpilation may omit type checking.
For boundary changes, test malformed input, absent fields, and failure variants;
a successful compile does not establish that a remote response matches a type.
