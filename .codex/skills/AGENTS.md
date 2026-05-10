# .codex/skills

## Purpose

Store repo-local skills that encode this repository's authoring workflow.

---

## Overview

### Major Contents

- `zenn-article-workflow/` - repo-local skill for drafting, polishing, and pre-publish review of Zenn articles

### Public Entrypoints

- repo-local skills discovered under this directory

### External Dependencies

- `.codex/config.toml`
- repository docs such as `docs/requirements.md` and `docs/agent-team/`

### Data Flow

User asks for repo-specific authoring help -> Codex reads the relevant skill under this directory -> the skill consults repository docs and article files.

---

## Requirements

- Keep each skill repo-specific; encode assumptions that are true for this repository.
- Store long references under each skill instead of bloating `SKILL.md`.

## Constraints

- Do not copy generic skills here unchanged; tailor them to this repo.
- Do not create repo-local skills that depend on secret credentials being present.

## Edit Notes

- When adding a new skill, keep its instructions aligned with `docs/requirements.md`, `docs/structure.md`, and `docs/agent-team/`.
