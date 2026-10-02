---
name: javafx-best-practices
description: Develop or review JavaFX desktop controls, presentation models, application threading, and modular packaging. Use for JavaFX UI and runtime changes, with Java guidance for language-level behavior.
---

# JavaFX best practices

Read the generated root [AGENTS.md](../../../AGENTS.md) for architecture,
pinned versions, module setup, and checks. Apply this technology guidance with
the assigned generic role; it neither launches agents nor expands write ownership.
Keep the established programmatic UI and presentation-model separation.

## UI lifecycle and concurrency

- Confine live scene-graph updates to the JavaFX Application Thread. Run slow
  I/O and computation in background work; batch UI updates instead of flooding
  the event queue. `Platform.runLater` queues UI work and does not make its
  body a background task. See [Platform threading](https://openjfx.io/javadoc/25/javafx.graphics/javafx/application/Platform.html).
- Prefer the existing `Task`/`Service` or executor boundary for background work.
  A `Task` is single-use and cancellation is cooperative; check cancellation
  during long operations and handle failure/cancellation states in the UI.
  Do not access live controls from `Task.call()`. See [Task lifecycle](https://openjfx.io/javadoc/25/javafx.graphics/javafx/concurrent/Task.html).
- Keep state transitions and decisions in display-independent presentation
  models. Do not require JavaFX toolkit startup merely to test parsing or
  business logic; adapt model outcomes to controls at the UI boundary.
- Release listeners, bindings, and owned background resources at the appropriate
  lifecycle boundary. Verify that closing a window cannot leave an unintended
  worker alive or enqueue updates to a disposed view.
- Update module declarations narrowly when dependencies change. Preserve the
  OpenJFX plugin's platform-specific runtime dependencies and the existing
  resource/CSS layout; avoid broad reflective opens to hide an access problem.

## Verification focus

Run display-independent tests through the root's headless checks. Use an actual
graphical session for changed layout, focus, keyboard access, and window lifecycle.
Test the supported target platform's distribution when packaging changes; a
successful headless build does not prove GUI behavior, bundled JDK availability,
or cross-platform native compatibility.
