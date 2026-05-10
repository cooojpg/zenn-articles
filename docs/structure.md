# Project Structure Definition

> Current adopted structure: Primary
> If you switch structures, update this line and the corresponding section.

This file is the source of truth for the repository structure.
Update this file before making structure-changing file operations.

---

## Fixed Scaffold

These files and directories are common regardless of the feature layout:

```text
.
|- .codex/
|  `- config.toml
|- docs/
|  |- requirements.md
|  `- structure.md
|- tasks/
|  |- in-progress.md
|  |- todo.md
|  `- issues.md
|- .gitattributes
`- AGENTS.md
```

The feature directories are described below.

---

## Primary Structure

```text
.
|- .codex/
|  |- AGENTS.md
|  |- config.toml
|  `- skills/
|     |- AGENTS.md
|     `- zenn-article-workflow/
|        |- AGENTS.md
|        |- SKILL.md
|        |- agents/
|        `- references/
|- articles/
|  |- *.md
|  `- image.png
|- books/
|  `- .keep
|- docs/
|  |- AGENTS.md
|  |- requirements.md
|  |- structure.md
|  `- agent-team/
|     |- AGENTS.md
|     |- agent-router.md
|     |- team-patterns.md
|     |- README.md
|     `- prompts/
|        |- core/
|        `- generated/
|- tasks/
|  |- AGENTS.md
|  |- in-progress.md
|  |- todo.md
|  `- issues.md
|- .gitattributes
|- AGENTS.md
|- README.md
`- LICENSE
```

**Rationale**

Keep the existing content-first layout (`articles/`, `books/`) intact, then add only the minimum Codex scaffold needed for repeatable authoring, review, and explicit multi-agent delegation. This avoids inventing new top-level content buckets before the repository actually needs them.

---

## Alternative Structure

```text
.
|- articles/
|- books/
|- research/
|- templates/
|- docs/
|- tasks/
|- .codex/
`- AGENTS.md
```

**Rationale**

This separates raw research notes and reusable templates from publishable article files. It can reduce clutter if the repository starts accumulating many research artifacts per article.

**Choose this when**

Use this only after research notes, reusable prompts, or shared templates become first-class artifacts that are too noisy to keep folded into `docs/` or ad hoc working notes.

---

## Key Decisions

- Keep published content under `articles/` rather than introducing a second draft tree.
- Reserve `.codex/skills/` for repo-local skills instead of mixing skill instructions into root docs.
- Treat `docs/agent-team/` as the durable entrypoint for delegation guidance.
- Keep `books/` intentionally light until the repository actually adopts a Zenn book workflow.
- Keep the current repository small; do not add deeper nesting without a concrete workflow need.

---

## `AGENTS.md` Placement Policy

- root `AGENTS.md` always exists
- each managed feature directory may contain its own `AGENTS.md`
- lower directories narrow scope and add detail

Exception:

- do not place `AGENTS.md` under Zenn-managed content roots such as `articles/` and `books/`

### Paths Intentionally Without `AGENTS.md`

- `articles/`
- `books/`
- `docs/agent-team/prompts/core/`
- `docs/agent-team/prompts/generated/`
- individual article files and image assets

---

## Agent Team Mapping

| ディレクトリ | 責務 | 関連要件 | Specialist prompt | 推奨 `agent_type` | 併用 Core prompts | 参加複雑度 |
|---|---|---|---|---|---|---|
| `articles/` | 記事テーマ整理と検索意図の定義 | 機能要件: 新規記事作成、slug 候補提案 | `docs/agent-team/prompts/generated/keyword-specialist.md` | `explorer` | `coordinator`, `planner` | M / L |
| `articles/` | 競合・一次情報の収集 | 機能要件: リサーチ支援、品質確認 | `docs/agent-team/prompts/generated/research-specialist.md` | `explorer` | `coordinator`, `planner`, `reviewer` | M / L / X |
| `articles/` | 記事構成と見出し設計 | 機能要件: 構成案提示 | `docs/agent-team/prompts/generated/outline-specialist.md` | `default` | `coordinator`, `planner` | M / L |
| `articles/` | 本文執筆と file edit | 機能要件: 新規記事作成、既存記事推敲 | `-` | `worker` | `implementer`, `reviewer` | S / M / L |
| `articles/` | 品質レビューと公開前チェック | 機能要件: 公開前チェック | `docs/agent-team/prompts/generated/quality-specialist.md` | `explorer` | `reviewer`, `coordinator` | M / L / X |
| `articles/` | 公開準備と `published` 切替判断 | 機能要件: 公開前チェック、公開準備 | `docs/agent-team/prompts/generated/publish-specialist.md` | `explorer` | `coordinator`, `implementer` | M / L |
| `articles/` | 公開後分析 | 機能要件: 分析エージェント | `docs/agent-team/prompts/generated/analytics-specialist.md` | `explorer` | `coordinator`, `planner` | L / X |
| `docs/agent-team/` | delegation 文書の保守 | ドキュメント整備、agent-team 運用 | `-` | `worker` | `docs-writer`, `reviewer` | M / L / X |

---

## Review Notes

- If a repo-local skill is added or materially changed under `.codex/skills/`, update this document and `docs/requirements.md` together.
- If publish automation starts talking to external systems, revisit the alternative structure and add explicit integration boundaries.

---

## Structure Change Procedure

1. Get explicit approval for the structure change
2. Update this file first
3. Then perform the file or directory changes
4. Update affected `AGENTS.md` files if the local meaning changed
