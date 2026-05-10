# docs/agent-team

## Purpose

Store the durable routing and prompt documents for explicit multi-agent article work in this repository.

---

## Overview

### Major Contents

- `agent-router.md` - entrypoint for deciding whether and how to use the agent team
- `team-patterns.md` - detailed orchestration patterns by task complexity
- `README.md` - quick index of generated prompts
- `prompts/core/` - core role prompts
- `prompts/generated/` - Zenn-specific specialist prompts

### Public Entrypoints

- `agent-router.md`
- `team-patterns.md`
- `README.md`

### External Dependencies

- root `AGENTS.md`
- `docs/requirements.md`
- `docs/structure.md`
- `.codex/config.toml`

### Data Flow

User explicitly requests delegation -> Codex reads `agent-router.md` -> selects the needed core and specialist prompts -> spawns only the required agents -> integrates and closes them.

---

## Requirements

- Keep `agent-router.md` as the entrypoint for delegation decisions.
- Keep prompt paths and role names synchronized across all files in this directory.

## Constraints

- Do not describe automatic delegation as the default path.
- Do not add worker prompts that edit the same target file in parallel.

## Edit Notes

- When prompt names change, update `agent-router.md`, `team-patterns.md`, `README.md`, and root `AGENTS.md` together.
