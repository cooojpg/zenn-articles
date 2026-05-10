# Project Instructions

This repository stores Zenn content and the repo-local authoring workflow that Codex uses to create and review that content.
Directory-specific details belong in the nearest child `AGENTS.md`.

---

## 1. File Change Policy

Normal edits, additions, and new files can proceed without prior approval.
Summarize the important change after the edit.

Ask before destructive or high-impact changes:

- deleting files or directories
- renaming existing article slugs in bulk
- rewriting `docs/structure.md` in a way that changes the approved structure
- removing existing content from `tasks/in-progress.md`
- loosening a rule in an existing child `AGENTS.md`
- destructive git operations
- intentionally changing established authoring conventions

## 2. Structure Source of Truth

- `docs/structure.md` is the source of truth for repository structure
- update `docs/structure.md` before creating, moving, or deleting structural directories
- keep child `AGENTS.md` files aligned with the structure document

## 3. Hierarchical `AGENTS.md`

Each managed directory may contain its own `AGENTS.md`.

Rules:

- higher levels define broader rules
- lower levels may add detail or narrower constraints
- lower levels should not silently loosen higher-level constraints
- before editing a file in a subtree, read the nearest applicable `AGENTS.md` chain if it is not already in context

## 4. Requirements

- `docs/requirements.md` is the source of truth for workflow and non-functional requirements
- read it before major workflow or content-process changes
- update it before changing the expected article lifecycle

## 5. Article Workflow

- `articles/` stores Zenn article Markdown files and related assets
- `articles/` and `books/` are Zenn-managed content roots, so do not place `AGENTS.md` or arbitrary Markdown files there unless they are valid Zenn content
- if `books/` is not in active use, keep a tracked placeholder such as `books/.keep` so the directory still exists on GitHub for Zenn
- new article filenames should use readable English `kebab-case` slugs
- default new article frontmatter should include `published: false`
- preserve existing random-style slugs unless the user explicitly asks to rename them
- use `type: "tech"` for technical walkthroughs and `type: "idea"` for opinion or reflection pieces

## 6. Task Tracking

Work state lives in `tasks/`:

- `tasks/in-progress.md` for active work
- `tasks/todo.md` for not-yet-started tasks
- `tasks/issues.md` for durable constraints, debt, or unresolved questions

Rules:

- read `tasks/in-progress.md` at the start of a new session
- move a task from `todo` to `in-progress` when starting it
- remove it from `in-progress` when done
- keep transient execution notes out of `tasks/issues.md`

## 7. Agent Team

This section applies only when the user explicitly asks for delegation, subagents, parallel agents, or an agent team.

- first read `docs/agent-team/agent-router.md`
- treat `docs/agent-team/agent-router.md` as the entrypoint and `docs/agent-team/team-patterns.md` as the detailed orchestration guide
- do not spawn subagents automatically for routine work; without explicit delegation, work locally and use the docs only as planning guidance
- never run two worker agents that edit the same file in parallel
- keep within `agents.max_threads = 12` and `agents.max_depth = 1`

Complexity mirror:

- `S`: one-file article tweak or typo fix
- `M`: new article or article plus one supporting doc update
- `L`: skill, docs, and workflow changes across multiple directories
- `X`: external publish or analytics automation that changes repo-wide rules

Generated Core prompts:

- `docs/agent-team/prompts/core/coordinator.md`
- `docs/agent-team/prompts/core/planner.md`
- `docs/agent-team/prompts/core/implementer.md`
- `docs/agent-team/prompts/core/reviewer.md`
- `docs/agent-team/prompts/core/docs-writer.md`

Generated Specialist prompts:

- `docs/agent-team/prompts/generated/keyword-specialist.md`
- `docs/agent-team/prompts/generated/research-specialist.md`
- `docs/agent-team/prompts/generated/outline-specialist.md`
- `docs/agent-team/prompts/generated/quality-specialist.md`
- `docs/agent-team/prompts/generated/publish-specialist.md`
- `docs/agent-team/prompts/generated/analytics-specialist.md`

## 8. Codex Repo Settings

- keep `.codex/config.toml` in the standard repo-local form
- keep the `[agents]` budget aligned with the agent-team docs

## 9. Cross-Tool Files

- if other tool-specific files are added later, treat them as supplemental
- do not delete or rewrite cross-tool files unless the user explicitly asks for that

## 10. Environment

- prefer LF line endings for shell scripts and repo metadata
- keep workflow instructions short and maintainable
- if a child `AGENTS.md` becomes stale, update it after the change that made it stale
