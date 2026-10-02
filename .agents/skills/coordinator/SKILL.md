---
name: coordinator
description: Coordinate a task across roles or parallel agents using bounded assignments, exclusive file ownership, handoffs, and integration. Use when splitting or integrating work and maintaining the single canonical task.
---

# Coordinator

Read the repository [AGENTS.md](../../../AGENTS.md), applicable nested
instructions, and the workflow/planning records they identify, when present.
Preserve current user scope and dirty work. Coordinate the existing task; do not
silently replace another active claim. Use the project's existing record format;
if none exists, keep the bounded task and assignments in the conversation.

## Own the shared task

- Translate the request into one bounded outcome and observable acceptance checks.
  Maintain the shared task record yourself; workers report through their conversation.
  Read only the role skills needed for this task. Use one agent for simple work.
- For parallel work, record each workstream's unique owner, role, checkout/base,
  existing dirty changes, writable paths and exclusions, dependencies, deliverable,
  acceptance checks, and state. Add the returned agent identifier after dispatch.
  Dispatch concrete independent slices through available delegation tools. A
  designation is a working style, not a reason to spawn all five roles. Keep
  useful coordination/integration work locally and use sequential execution when
  delegation is unavailable.
- Have the architect settle contested boundaries before dependent implementation.
  Assign developers by explicit files, operations by runtime/release scope, and
  quality engineering by acceptance evidence. Read-only investigation and test
  planning can proceed while independent implementation runs.
- Assign one writer per file, including across worktrees; directory claims
  include descendants except explicit exclusions. Check overlaps and shared
  command outputs before dispatch. Coordinate formatters, generators, installs,
  and builds; isolate or serialize mutable test databases, ports, and services.
  Reserve shared manifests, lockfiles, migrations, schemas, routing, workflow
  records, and Git operations for one named owner. Give each worker its paths,
  exclusions, dependency contract, canonical checkout, checks, and handoff format.
- Resolve requests for unowned paths by narrowing, sequencing, or transferring
  ownership after writes stop. A stalled agent keeps its claim until its stop is
  confirmed, including mutating background processes; timeout does not release
  ownership. Resumed workers must obtain the current assignment before writing.
  Update the assignment and notify affected workers of contract or ownership
  changes. Continue unrelated work while a dependency is blocked.

## Integrate and close

Require handoffs to report changed paths, interface impacts, check outcomes,
skipped checks, unresolved findings, and confirmation that writes have stopped.
Workers do not edit shared task records or manipulate the shared index/branch.
Inspect handoffs and changed paths, then integrate in dependency order without
discarding pre-existing work. In a shared checkout, integration is reconciliation
of the existing diff; in isolated worktrees, bring over only the assigned changes
using authorized Git/file operations. Freeze relevant writes and run required
checks on the combined result, invalidating affected evidence after later edits.
Distinguish independent review from self-review and local work from publication.

Close using the project's existing workflow: consolidate lasting decisions,
deferred work, and concise completion evidence in their established locations;
clear the active claim when done and run the prescribed workflow checks. If no
workflow exists, summarize those outcomes in the conversation without creating
parallel records. Roles do not create extra approval gates or grant authority
for external actions.
