---
name: rust-best-practices
description: Apply Rust ownership, error, and API guidance to Rust libraries and executable adapters. Use for Rust implementation and review; combine with Axum guidance for HTTP services.
---

# Rust best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned toolchain, Cargo policy, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Honor the configured edition, minimum Rust version, features, and lint policy.

## Ownership and APIs

- Express ownership at interfaces: borrow read-only inputs when callers retain
  them, and take ownership when storing or transferring a value. Avoid adding
  clones, shared ownership, or interior mutability just to silence a borrow
  error; first shorten the lifetime or separate the operation.
  See [references and borrowing](https://doc.rust-lang.org/book/ch04-02-references-and-borrowing.html).
- Return `Result` for expected input, I/O, and dependency failures. Preserve
  typed causes for callers; add context where the operation is known. Reserve
  panics for violated internal invariants consistent with repository policy,
  never ordinary malformed user input. See [error handling](https://doc.rust-lang.org/book/ch09-00-error-handling.html).
- Keep reusable behavior in library modules and command parsing, process exits,
  or HTTP status mapping in adapters. Public enum variants and serialized shapes
  are compatibility surfaces, even when an internal refactor compiles.
- For async code, review ownership across suspension and cancellation. Keep
  ordinary mutex guards out of `.await` spans; use a short critical section or
  an appropriate async synchronization boundary when waiting is necessary.
  See [Tokio shared state](https://tokio.rs/tokio/tutorial/shared-state).
- Keep unsafe-code restrictions intact. Do not add an unsafe shortcut or widen
  dependency features to bypass an unexplained compiler constraint.

## Verification focus

Run the root's formatting, lint, tests, and applicable feature combinations.
Test errors and public behavior, including exit status and output streams for CLI
changes. Test async shutdown or cancellation when changed; compilation alone
does not prove a task releases resources or stops accepting work correctly.
