# Smithy source and technology selection

This public distribution uses the workflow and skill content shipped in
[Alconite CLI 2026.10.1](https://github.com/alconite-inc/alconite-cli/releases/tag/v2026.10.1),
profile `alconite-workflow-v3`. The source snapshot is Alconite's Smithy generator
revision `4dd760417eb1136aa78c95034db48fe7e43fe2d1`.

## Source map

Paths below identify the generator source tree, not files consumers must install
or an additional dependency on that source repository.

| Public content | Smithy source | Transformation |
| --- | --- | --- |
| Five role `SKILL.md` files | `crates/project_generator/workflows/alconite-workflow-v3/.agents/skills/` | Copied byte for byte |
| Thirteen technology `SKILL.md` files | `crates/project_generator/technology-skills/` | Copied byte for byte into `.agents/skills/` |
| Workflow README, idle task, empty history | `crates/project_generator/workflows/alconite-workflow-v3/.alconite/workflow/` | Copied byte for byte |
| Overview, project plan, build plan | The same profile directory's three `.md.j2` files | Rendered `project.display` as `Project`; no renderer needed by consumers |
| `workflow.json` | `src/workflow/agent_skills.rs` and `compact.rs` in `crates/project_generator` | Standard v3 configuration with project ID `agent-workflow` and all 13 supported technologies |
| Root workflow routing | `agents_pointer` in `src/workflow/agent_skills.rs` | Standard v3 section with all technologies, following a portable project-instruction preface |

The root README and migration guide describe this public packaging. The shipped
workflow contract and skill bodies retain Smithy's wording so the public content
and native validation stay aligned. This is a content snapshot, not a runtime
dependency or an automatic synchronization job.

The previous public distribution remains available in this repository's Git
history at `e115b28`. Its design-provenance note belongs to that legacy pack; this
replacement comes from Alconite's Smithy v3 sources. No unrelated application,
customer records, deployment state, or generator implementation is included.

## Generated starter selection

All 15 starters in this snapshot include the five generic roles. Their template
manifests explicitly select the following technology skills; selection is not
inferred from a project name or installed dependencies. Each technology maps to
`.agents/skills/<technology>-best-practices/SKILL.md`.

| Starter ID | Technologies |
| --- | --- |
| `rust-cli` | Rust |
| `rust-axum-web` | Axum, Rust |
| `rust-axum-react` | Axum, React, Rust, TypeScript |
| `rust-axum-qwik` | Axum, Qwik, Rust, TypeScript |
| `spring-boot` | Java, Spring Boot |
| `spring-boot-modular-api` | Java, Spring Boot |
| `spring-boot-kafka-worker` | Java, Kafka, Spring Boot |
| `spring-boot-astro` | Astro, Java, Spring Boot, TypeScript |
| `spring-boot-nextjs` | Java, Next.js, React, Spring Boot, TypeScript |
| `spring-boot-qwik` | Java, Qwik, Spring Boot, TypeScript |
| `helidon-se-api` | Helidon SE, Java |
| `helidon-mp-api` | Helidon MP, Java |
| `javafx-gradle` | Java, JavaFX |
| `nextjs-web` | Next.js, React, TypeScript |
| `qwik-web` | Qwik, TypeScript |

To create a new application with its matching subset, use a compatible CLI, for
example:

```bash
alconite smithy new rust-axum-react "My Application" --output /absolute/path/to/projects
```

The generated application also includes stack-specific root instructions and
application files; this repository distributes the reusable instructions and
workflow only. Consumers copying this public pack retain the full catalog but
load only relevant skills. Arbitrarily deleting skills or changing the selection
without updating the matching configuration and exact routing breaks validation.

## Maintaining the distribution

Update from an identified Smithy source revision and inspect the diff against
the released profile. Preserve the immutable workflow contract for that profile;
a contract change should arrive as a versioned successor. Keep baseline plans
generic, `active-task.md` idle, and `history.json` empty so maintenance work is
not copied into consumer projects. Record distribution changes and verification
in commits and pull requests instead.

Before publishing, compare copied skills and static workflow files with the
source snapshot, confirm the starter mapping against its manifests, check local
links, and validate the public root with the native CLI. Exercise the documented
copy into a disposable repository with existing root instructions and an unrelated
skill, then validate that consumer. These checks establish distribution integrity;
they do not prove agent behavior, native application builds, or live deployments.
