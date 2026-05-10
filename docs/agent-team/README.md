# Agent Team README

`docs/agent-team/` は、この Zenn リポジトリで明示 delegation を行うときの運用資料である。

## Entry Point

- 最初に `docs/agent-team/agent-router.md` を読む
- 詳細な起動パターンは `docs/agent-team/team-patterns.md` を読む

## Delegation Preconditions

- ユーザーが delegation / subagents / parallel agents / agent team を明示的に要求している
- write scope が衝突しない
- `agents.max_threads = 12` と `agents.max_depth = 1` を守る

## Standard Lifecycle

1. 整理
2. 作業
3. レビュー / 検証
4. 完了処理

blocking finding が出た場合は、Phase 2b 修正作業 -> Phase 3b 再レビュー / 再検証を回す。

## Core Prompts

- `docs/agent-team/prompts/core/coordinator.md` - `default`
- `docs/agent-team/prompts/core/planner.md` - `default`
- `docs/agent-team/prompts/core/implementer.md` - `worker`
- `docs/agent-team/prompts/core/reviewer.md` - `explorer`
- `docs/agent-team/prompts/core/docs-writer.md` - `worker`

## Specialist Prompts

- `docs/agent-team/prompts/generated/keyword-specialist.md` - `explorer`
- `docs/agent-team/prompts/generated/research-specialist.md` - `explorer`
- `docs/agent-team/prompts/generated/outline-specialist.md` - `default`
- `docs/agent-team/prompts/generated/quality-specialist.md` - `explorer`
- `docs/agent-team/prompts/generated/publish-specialist.md` - `explorer`
- `docs/agent-team/prompts/generated/analytics-specialist.md` - `explorer`

## User-Facing Mapping

- KW選定エージェント -> `keyword-specialist`
- リサーチエージェント -> `research-specialist`
- 設計エージェント -> `outline-specialist` + `planner`
- 執筆エージェント -> `implementer`
- 品質エージェント -> `quality-specialist` + `reviewer`
- 公開エージェント -> `publish-specialist` + 必要時 `implementer`
- 分析エージェント -> `analytics-specialist`
