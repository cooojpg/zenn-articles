# .codex

## Purpose

Store repo-local Codex configuration and repo-specific skills.

---

## Overview

### Major Contents

- `config.toml` - repo-local Codex defaults, including sandbox and agent limits
- `skills/` - reserved location for repo-specific skills such as the Zenn workflow skill

### Public Entrypoints

- `config.toml` - loaded as repo-local Codex configuration

### External Dependencies

- Codex runtime

### Data Flow

Codex enters the repository -> loads `.codex/config.toml` -> optionally reads repo-local skills under `.codex/skills/`.

---

## Requirements

- Keep repo-local Codex settings minimal and explicit.
- Keep agent budget in sync with `docs/agent-team/`.

## Constraints

- Do not broaden sandbox or approval settings without explicit user approval.
- Do not store credentials or tokens in this directory.

## Edit Notes

- If `agents.max_threads` or `agents.max_depth` changes, update `docs/agent-team/agent-router.md`, `docs/agent-team/team-patterns.md`, and `AGENTS.md` in the same task.
