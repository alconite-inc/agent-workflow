# Agent Workflow

Agent Workflow is a copy-ready, repository-local workflow for planning,
implementing, reviewing, and recording agent-assisted development. It uses
ordinary Markdown and JSON files—no installer, Jinja renderer, hooks, runtime,
or package-manager dependencies are required.

## What to copy

The reusable pack consists of two paths:

- `.workflow/` contains the plans, workflow state, schemas, history, and
  release records.
- `.agents/skills/agent-workflow-*` contains the agent skills for each
  workflow phase.

Do not copy this repository's `.git/` directory. The empty `.codex/` directory
is not part of the pack.

## Copy into a new project

Run these commands from any shell with `rsync` installed. Set both paths to
absolute locations:

```bash
SOURCE_ROOT=/absolute/path/to/agent-workflow
TARGET_ROOT=/absolute/path/to/target-project

test -d "$SOURCE_ROOT/.workflow"
test -d "$TARGET_ROOT/.git"
test ! -e "$TARGET_ROOT/.workflow"

mkdir -p "$TARGET_ROOT/.agents/skills"

rsync -a --dry-run --itemize-changes \
  "$SOURCE_ROOT/.workflow/" \
  "$TARGET_ROOT/.workflow/"

rsync -a --dry-run --itemize-changes \
  "$SOURCE_ROOT/.agents/skills/" \
  "$TARGET_ROOT/.agents/skills/"
```

Inspect the dry-run output. If the paths are correct, repeat the two `rsync`
commands without `--dry-run`:

```bash
rsync -a --itemize-changes \
  "$SOURCE_ROOT/.workflow/" \
  "$TARGET_ROOT/.workflow/"

rsync -a --itemize-changes \
  "$SOURCE_ROOT/.agents/skills/" \
  "$TARGET_ROOT/.agents/skills/"
```

The copied `.workflow/` directory already contains ordinary, usable files:

- `project-plan.md`
- `build-plan.md`
- `project-overview.md`
- `workflow.json`

Set `projectId` in `.workflow/workflow.json` to the target repository's
kebab-case name. This is the only required project-specific substitution; it
is a normal JSON edit, not a rendering step.

Keep the target project's root `AGENTS.md`; it should define project-specific
architecture, ownership, security constraints, and verification commands. Use
the first build-plan item to review and tailor the starter plans through the
normal workflow.

## Use the workflow

Open the target project as the agent's repository root. The normal lifecycle
is:

```text
agent-workflow-start
  -> agent-workflow-discover
  -> agent-workflow-specify
  -> human specification approval
  -> agent-workflow-implement
  -> agent-workflow-review
  -> agent-workflow-complete
```

Use `agent-workflow-status` or `agent-workflow-brief` to inspect current state
without changing it. Additional skills cover diagnosis, overview refresh,
release readiness, and recording separately authorized release evidence.

See [the workflow reference](.workflow/README.md) for lifecycle rules,
authority boundaries, and state ownership.

## Update an existing installation

Do not replace an existing `.workflow/` directory with this repository's
baseline copy. That can overwrite active tasks, project plans, history,
indices, release evidence, or project-specific hashes.

Update the agent skills independently and inspect the dry run before copying:

```bash
SOURCE_ROOT=/absolute/path/to/agent-workflow
TARGET_ROOT=/absolute/path/to/target-project

rsync -a --dry-run --itemize-changes \
  "$SOURCE_ROOT/.agents/skills/" \
  "$TARGET_ROOT/.agents/skills/"
```

Changes to `.workflow/` schemas or static documents should be handled as an
intentional migration that preserves the destination project's state.

## Verify the installation

From the target project root:

```bash
test -f .workflow/workflow.json
test -f .workflow/project-plan.md
test -f .workflow/project-overview.md
test -f .agents/skills/agent-workflow-start/SKILL.md

find .agents/skills -maxdepth 1 -type d \
  -name 'agent-workflow-*' -print | sort

git status --short
```

Review the copied files before committing them. Workflow records never grant
authority to commit, push, merge, release, deploy, or perform another external
action.
