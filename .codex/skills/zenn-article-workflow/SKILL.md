---
name: zenn-article-workflow
description: Repo-local workflow for drafting, polishing, and pre-publish review of Zenn articles in this repository. Use when Codex needs to create or edit `articles/*.md`, turn rough notes into a Zenn article, propose readable English slug candidates, run a publish-readiness check, or coordinate the documented article agent-team when the user explicitly asks for delegation.
---

# Zenn Article Workflow

Read the repository rules first:

- `AGENTS.md`
- `articles/AGENTS.md`
- `docs/requirements.md`
- `docs/structure.md`

Read the bundled references as needed:

- `references/style.md` for writing style and article structure
- `references/checklist.md` for publish-readiness checks

If the user explicitly asks for delegation, subagents, parallel agents, or an agent team, read `docs/agent-team/agent-router.md` and follow that routing. Do not spawn subagents by default.

## New Article Workflow

1. Start from the user's theme, memo, or rough bullets.
2. If the input is thin, ask 3 to 5 short questions covering:
   - the main conclusion
   - what was difficult or surprising
   - the target reader
   - whether the article should be `tech` or `idea`
   - whether this is still a draft or intended as a publish candidate
3. Propose 2 to 3 readable English `kebab-case` slug candidates.
4. Avoid machine romaji conversion. Prefer meaning-based English phrases.
5. Before finalizing a slug, check existing filenames under `articles/` and avoid collisions.
6. Propose the outline before writing the full article.
7. After the direction is aligned, create `articles/<slug>.md`.
8. Keep frontmatter keys in this order:

```yaml
title: ""
emoji: ""
type: "tech"
topics: []
published: false
```

9. Default to `published: false` unless the user explicitly asks to publish now.

## Structure Rules

Use the article type to choose the baseline structure.

For `tech`:

1. 背景
2. 何に詰まったか
3. 調査・検証
4. 結論 / 学び

For `idea`:

1. きっかけ
2. 問題意識
3. 判断
4. 学び / 共有したいこと

Use `references/style.md` to adapt the tone and section wording to this repository's existing articles.

## Existing Article Workflow

- Edit the requested article file directly.
- Preserve existing frontmatter keys unless a change is requested or clearly necessary.
- Keep the article's main claim sharper after the edit than before it.
- Summarize the meaningful changes after editing.

## Pre-Publish Review

- Use `references/checklist.md`.
- Check title, emoji, `type`, topics, heading flow, links, code blocks, and factual confidence.
- If `published` should remain `false`, leave it unchanged.
- If the user explicitly asks to publish, perform the final review first and only then switch `published` to `true`.

## Delegation Rules

- Delegation is opt-in only.
- When delegation is requested, use `docs/agent-team/agent-router.md` as the entrypoint.
- Typical pattern for a new article:
  - `keyword-specialist` + `research-specialist`
  - `planner` / `outline-specialist`
  - `implementer`
  - `reviewer` / `quality-specialist`
- Never run two worker agents that edit the same article file in parallel.

## Non-Goals

- Do not auto-post to WordPress or another CMS from this skill.
- Do not claim ranking or traffic analysis without user-provided metrics or a configured data source.
- Do not bulk-rename existing article slugs unless the user explicitly asks for migration.
