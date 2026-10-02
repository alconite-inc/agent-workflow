# Agent Workflow

Copy-ready agent roles, technology guidance, and compact workflow records from
Alconite Smithy. This distribution follows `alconite-workflow-v3`, shipped with
[Alconite CLI 2026.10.1](https://github.com/alconite-inc/alconite-cli/releases/tag/v2026.10.1).
It uses ordinary Markdown and JSON; copying and using the pack requires no
renderer, installer, hooks, runtime service, or package-manager dependencies.
The optional native validator requires a compatible Alconite CLI.

## What is included

```text
AGENTS.md                         Root routing and project instructions
.agents/skills/<name>/SKILL.md     Five roles and 13 technology skills
.alconite/workflow/
  README.md                       Versioned operating contract
  workflow.json                   Profile identity, selection, and limits
  project-overview.md              Architecture and lasting decisions
  project-plan.md                  Purpose, scope, and success criteria
  build-plan.md                   Ordered milestones
  active-task.md                  One current task, or none
  history.json                    At most 20 completion summaries
```

This replaces the earlier `.workflow/` directory, phase-specific
`agent-workflow-*` skills, schema files, and per-task archives. Existing users
should follow the [migration guide](docs/migration.md) before adopting it.

## Roles and technology guidance

Roles describe how an agent works. The assignment defines which files it owns.
One agent can use several roles sequentially; load only what the task needs.

| Role | Responsibility |
| --- | --- |
| [Architect](.agents/skills/architect/SKILL.md) | Boundaries, interfaces, compatibility, and dependency order |
| [Developer](.agents/skills/developer/SKILL.md) | An assigned implementation slice and focused tests |
| [Operations](.agents/skills/operations/SKILL.md) | Runtime, CI, packaging, delivery, and recovery |
| [Quality engineer](.agents/skills/quality-engineer/SKILL.md) | Acceptance checks, regressions, and integration evidence |
| [Coordinator](.agents/skills/coordinator/SKILL.md) | Assignments, file ownership, handoffs, integration, and closure |

The technology catalog covers Astro, Axum, Helidon MP, Helidon SE, Java, JavaFX,
Kafka, Next.js, Qwik, React, Rust, Spring Boot, and TypeScript. Pair the relevant
language and framework guidance with a role: for example, developer + Java +
Spring Boot, or quality engineer + Rust + Axum.

This public pack includes all 13 technologies so it can be copied without a
generator. Its configuration and root routing declare that full catalog; only
load the skills relevant to the actual project. Smithy-generated applications
receive a selected subset instead. See the [starter mapping](docs/smithy.md).
The skills preserve starter-specific assumptions, so compare them with the
project's actual architecture and pinned versions before applying them.

## Copy into a project without a workflow

Start with a clone of this repository. Set the source and target to absolute
paths. The following subshell checks for existing workflow state and skill-name
collisions, creates parent directories, and previews the copy with `rsync`:

```bash
(
  set -eu
  SOURCE_ROOT=/absolute/path/to/agent-workflow
  TARGET_ROOT=/absolute/path/to/target-project

  test -f "$SOURCE_ROOT/.alconite/workflow/workflow.json"
  test -e "$TARGET_ROOT/.git"
  test ! -e "$TARGET_ROOT/.workflow"
  test ! -e "$TARGET_ROOT/.alconite/workflow"
  for skill in "$SOURCE_ROOT"/.agents/skills/*; do
    test ! -e "$TARGET_ROOT/.agents/skills/${skill##*/}"
  done

  mkdir -p "$TARGET_ROOT/.alconite" "$TARGET_ROOT/.agents/skills"
  rsync -a --dry-run --itemize-changes \
    "$SOURCE_ROOT/.alconite/workflow/" "$TARGET_ROOT/.alconite/workflow/"
  rsync -a --dry-run --itemize-changes \
    "$SOURCE_ROOT/.agents/skills/" "$TARGET_ROOT/.agents/skills/"
)
```

Inspect the output, resolve any failed precondition, then repeat the whole block
with `--dry-run` removed from both commands. Do not bypass a collision by
overwriting project-owned skills; reconcile the existing content first.
The same directories can be copied with a file manager, including hidden files.

Finish the integration before starting an agent:

1. If the target has no root `AGENTS.md`, copy this pack's [AGENTS.md](AGENTS.md)
   and fill in its project instructions. Otherwise preserve the existing file
   and append the complete `## Alconite Workflow` section from this pack, with a
   blank line before its heading. Keep exactly one such section at the end.
   Put all project-specific instructions above it; the validator expects the
   versioned section unchanged, including its final newline.
2. Set `projectId` in `.alconite/workflow/workflow.json` to the target project's
   lowercase kebab-case name. Preserve the profile, selection, and limits.
3. Refine the overview, project plan, and first build-plan milestone for the
   actual project. The baseline has no active task and an empty history.
4. Validate and review the resulting diff before committing.

Copy only `.alconite/workflow/`, `.agents/skills/`, and the root instruction
integration. This repository's `.git/` directory and `docs/` are not part of a
consumer installation. Do not replace an existing Smithy-generated v3 workflow
with this baseline: its selected technologies and working records already belong
to that project.

## Work and consolidate

Read the overview, project plan, build plan, active task, and workflow contract.
Claim an unfinished milestone such as `APP-01` with one task:

```text
Task: add-health-endpoint
Milestone: APP-01
Status: implementing
```

Keep scope, acceptance criteria, findings, and concise verification evidence in
that same `active-task.md`. Status may be `planned`, `implementing`, `verifying`,
or `blocked`. Existing user authorization continues to apply; these records do
not create repeated approval gates.

For parallel work, one coordinator records each owner's checkout, writable
paths, exclusions, dependencies, deliverable, checks, and state. Each file has
one writer, including across worktrees. Stop writes and owned mutating processes
before handing off; a timeout does not release ownership. Verify the combined
result before closure. These instructions do not spawn agents or enforce locks.

At completion, move lasting decisions into the overview, deferred work into the
build plan, and one short completion record into `history.json`. Check the
finished milestone. Replace the active task with the next one, or set all three
fields to `none`. Keep at most 20 history entries and confirm old detail is
committed before pruning it; Git cannot recover uncommitted work.

See the [workflow contract](.alconite/workflow/README.md) for exact history fields,
budgets, and coordination rules. Keep the seven-file structure; do not add logs,
dated specifications, revision folders, or task archives there.

## Validate and maintain

With [Alconite CLI 2026.10.1](https://github.com/alconite-inc/alconite-cli/releases/tag/v2026.10.1)
or a later CLI supporting this profile, run from the target root:

```bash
alconite --version
alconite smithy workflow validate --root .
git diff --check
git status --short
```

The equivalent standalone command is
`alconite_smithy workflow validate --root .`. The full public pack reports
`alconite-workflow-v3`, 25 managed files, and zero active tasks before use.
Validation checks structure, identities, bounds, task claims, and required skill
frontmatter; it does not verify that recorded evidence is true or replace the
project's native tests. It reads the explicit root without running project code.

Keep the workflow README and root workflow section unchanged. Evolve the five
working records and skill bodies within the profile limits. Preserve skill
frontmatter identity and the required catalog; removing a declared skill fails
validation. Extra project-owned skills can coexist. Apply updates as reviewed
diffs, preserving consumer edits. Distribution provenance and the Smithy source
mapping are in [docs/smithy.md](docs/smithy.md).
