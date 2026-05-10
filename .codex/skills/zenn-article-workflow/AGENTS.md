# zenn-article-workflow

## Purpose

Define the repo-local Codex skill for drafting, polishing, and pre-publish review of Zenn articles in this repository.

---

## Overview

### Major Contents

- `SKILL.md` - main skill instructions and trigger description
- `agents/openai.yaml` - UI-facing metadata for the skill
- `references/style.md` - writing and structure guidance
- `references/checklist.md` - publish-readiness checklist

### Public Entrypoints

- `SKILL.md`

### External Dependencies

- `AGENTS.md`
- `docs/requirements.md`
- `docs/structure.md`
- `docs/agent-team/agent-router.md`

### Data Flow

User asks for article help -> skill triggers -> Codex reads `SKILL.md` and the needed references -> edits `articles/*.md` or related docs.

---

## Requirements

- Keep the skill repo-specific.
- Keep `SKILL.md` concise and push detailed guidance into `references/`.

## Constraints

- Do not assume external publishing automation exists.
- Do not instruct automatic delegation by default.

## Edit Notes

- If article conventions change, update `references/style.md` and `references/checklist.md` in the same task.
- If agent-team routing changes, update the delegation section in `SKILL.md`.
