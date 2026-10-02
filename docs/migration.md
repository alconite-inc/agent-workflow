# Migrate an existing workflow

This is a deliberate migration, not an in-place `rsync` update. The old public
pack stores canonical state in `.workflow/`; Smithy v3 uses
`.alconite/workflow/`. A fresh copy of the new baseline cannot represent the
existing project's active work, history, release evidence, or custom skills.

## From the earlier public pack

1. Inspect the checkout, existing dirty changes, workflow state, root instructions,
   and customized `agent-workflow-*` skills. Stop or hand off active writers before
   moving canonical state. Preserve uncommitted work and evidence before replacing
   any file. Confirm retained historical content is committed and reachable in Git.
2. Prepare the new seven-file directory alongside the old state while reconciling
   it, with one designated owner. Keep using only one canonical task during the
   migration. Set the new project's identity and retain the v3 contract and limits.
3. Carry current architecture and lasting decisions into `project-overview.md`,
   intent and acceptance criteria into `project-plan.md`, and unfinished work into
   `build-plan.md`. Translate old IDs such as `AW-BP-001` into unique v3 milestone
   IDs such as `APP-01`, updating references together. Keep only current content.
4. Translate the one active task into `active-task.md`, or use `none` for all three
   fields when idle. Preserve its scope, findings, dependencies, actual evidence,
   and any ownership assignments. Do not map a pending decision to completed work.
5. Summarize at most 20 recent completions in `history.json`, oldest first. Follow
   the exact fields and limits in the [v3 contract](../.alconite/workflow/README.md).
   Keep full legacy history and release/operation records in reachable Git history
   or their established project-owned evidence location outside the new workflow.
   Do not invent commit hashes, approvals, independent review, or verification.
6. Reconcile the five role skills and relevant technology guidance with existing
   project-owned skills. The public baseline declares the entire catalog; keep its
   required skills, configuration, and root routing together. Preserve specialized
   instructions under nonconflicting names where they remain useful. Remove the
   superseded `agent-workflow-*` adapters from active skill discovery after their
   project-specific guidance is carried forward. Smithy's older
   `alconite-workflow-*` adapters must also be removed for native v3 validation.
7. Preserve the root `AGENTS.md` project instructions and replace any old workflow
   routing with this pack's final `## Alconite Workflow` section. Update CI, scripts,
   documentation, and agent references that still use the old directory or phase
   skills. V3 has no phase `skill.json` manifests or per-task JSON schemas.
8. Once reconciliation is complete and recoverability is confirmed, remove the
   obsolete `.workflow/` tree from the current checkout. Do not leave two active
   systems for agents to follow. Review the complete migration diff and run
   `alconite smithy workflow validate --root .` with a CLI supporting v3, plus the
   project's relevant checks. Commit the coherent migration under the existing
   authorization.

The old specification-digest and checkpoint lifecycle is not part of v3. The new
contract retains user scope, concise evidence, and explicit ownership while
updating current documents in place. Carry forward any project-specific review
or release requirements in root instructions or the appropriate existing runbook.
An existing approval remains effective; workflow files do not grant authority.

## From a Smithy-generated project

For v1 or v2, preserve the same working information and prepare a reviewed
conversion to v3; changing only the profile string is insufficient. The README,
configuration, root routing, and required skills must agree with the v3 contract.

For an existing v3 project, keep its generated technology selection and canonical
records. Update only the relevant skill bodies as reviewed diffs, preserving
consumer changes and frontmatter. Do not copy this catalog's `workflow.json` or
full root routing over a project that uses a smaller generated selection. Run the
native validator after workflow or skill changes. A newly generated disposable
project using the same starter can serve as a comparison without overwriting the
existing application.
