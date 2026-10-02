---
name: java-best-practices
description: Apply Java language guidance to Java source, value models, resource handling, and concurrency. Combine with the relevant framework skill for Spring, Helidon, or JavaFX behavior.
---

# Java best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, build wrappers, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Use documentation matching the project's toolchain before adopting newer APIs.

## Implementation boundaries

- Represent meaningful states with explicit types instead of loosely related
  strings or booleans. Records suit value carriers, but final component fields
  do not make referenced collections immutable; copy mutable inputs when value
  isolation matters. Check generated equality and `toString()` before using a
  record for arrays or sensitive data. See [record classes](https://docs.oracle.com/en/java/javase/25/language/records.html).
- Close owned streams, files, and other `AutoCloseable` resources with
  try-with-resources. Do not close resources whose lifecycle belongs to a caller
  or container. Preserve the original failure and suppressed close failures.
  See [resource management](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html).
- Make nullability and collection mutability explicit at module boundaries.
  Validate external data before constructing domain values; avoid exposing a
  mutable internal collection through an accessor.
- Preserve exception causes when translating errors at an adapter. Avoid broad
  catches that silently convert programming failures into successful defaults.
  Propagate interruption where possible; when catching it without propagating,
  preserve the cancellation signal according to the executor's contract.
- Keep framework lifecycle and transport annotations out of any plain Java
  domain layer the project defines. Do not introduce preview features or a
  newer language level merely because an example uses them.

## Verification focus

Use the root's wrapper and checks. Exercise value invariants, malformed inputs,
resource cleanup on failures, and exception translation in plain unit tests.
For concurrency changes, verify shutdown and cancellation with bounded waits;
avoid tests whose correctness depends on a fixed sleep or shared mutable state.
