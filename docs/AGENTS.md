# docs

## Purpose

Store durable workflow documentation, structure rules, and agent-team guidance for this repository.

---

## Overview

### Major Contents

- `requirements.md` - source of truth for workflow requirements
- `structure.md` - source of truth for repository layout
- `agent-team/` - delegation guidance and prompt templates

### Public Entrypoints

- `requirements.md`
- `structure.md`
- `agent-team/agent-router.md`

### External Dependencies

- root `AGENTS.md`
- `.codex/config.toml`

### Data Flow

Repository conventions are decided -> documented under `docs/` -> consumed by Codex and repo-local skills during later work.

---

## Requirements

- Keep `requirements.md`, `structure.md`, and `agent-team/` consistent with each other.
- Record durable process changes here instead of in `tasks/`.

## Constraints

- Do not change repository structure without updating `structure.md` first.
- Do not document automatic subagent spawning; delegation must remain explicit.

## Edit Notes

- When adding a new process document, link it from the nearest entrypoint document.
- If a prompt list changes, update both `agent-router.md` and `README.md` under `agent-team/`.
