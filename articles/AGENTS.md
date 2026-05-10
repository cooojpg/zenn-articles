# articles

## Purpose

Store Zenn article source files and small article-local assets.

---

## Overview

### Major Contents

- `*.md` - article source files with Zenn frontmatter and Japanese body text
- `image.png` - article asset currently stored in this directory

### Public Entrypoints

- Markdown files under this directory are the publishable content consumed by Zenn

### External Dependencies

- Zenn article frontmatter conventions
- repository-wide style and checklist documents

### Data Flow

Topic or memo -> article draft Markdown -> review and publish preparation -> `published` switch when the article is ready.

---

## Requirements

- Keep frontmatter keys in the order `title`, `emoji`, `type`, `topics`, `published`.
- Use readable English `kebab-case` slugs for new article filenames.
- Default new files to `published: false` unless the user explicitly asks to publish.
- Keep article body readable in Japanese and structure it around a clear lesson or conclusion.

## Constraints

- Do not bulk-rename existing article files unless the user explicitly asks for migration.
- Do not add unsupported Zenn frontmatter keys casually.
- Do not leave placeholder text such as `TODO` in publish-ready article bodies.

## Edit Notes

- Check title, emoji, topics, and `type` after every meaningful article edit.
- If a change affects style conventions, update `docs/agent-team/` or the future repo-local skill references in the same task.
