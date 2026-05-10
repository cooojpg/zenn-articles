# tasks

## Purpose

Track current work, backlog items, and durable workflow issues.

---

## Overview

### Major Contents

- `in-progress.md` - active tasks only
- `todo.md` - queued work
- `issues.md` - durable issues, tradeoffs, and open questions

### Public Entrypoints

- `in-progress.md`
- `todo.md`
- `issues.md`

### External Dependencies

- `docs/requirements.md`
- `docs/structure.md`

### Data Flow

Planned work enters `todo` -> active work moves to `in-progress` -> durable fallout moves to `issues`.

---

## Requirements

- Keep entries short, concrete, and current.
- Preserve durable issues until they are truly resolved.

## Constraints

- Do not treat `issues.md` as a transient scratchpad.
- Do not leave completed tasks in `in-progress.md`.

## Edit Notes

- If a task changes scope materially, update its requirement or structure reference too.
