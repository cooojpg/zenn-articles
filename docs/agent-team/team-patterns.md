# Zenn Article Team Patterns

この文書は `docs/agent-team/agent-router.md` の詳細版であり、明示 delegation 時の起動パターンを定義する。

## Operating Rules

- 明示 delegation がないときは使わない
- 同一ファイルを触る worker 並列は禁止
- `fork_context` は必要最小限にし、長い会話履歴よりも対象ファイルと brief を優先する
- `agents.max_threads = 12`、`agents.max_depth = 1` を超えない

---

## Lifecycle and Phase Gates

1. 整理
2. 作業
3. レビュー / 検証
4. 完了処理

Phase gate:

- Phase 1 で対象ファイル、記事タイプ、未解決事項、write scope を固定する
- Phase 2 では `implementer` か `docs-writer` の ownership を明示する
- Phase 3 では `reviewer` / `quality-specialist` が blocking / follow-up / accepted risk を分類する
- blocking finding がある限り、Phase 2b 修正作業 -> Phase 3b 再レビュー / 再検証を回す

---

## Tool Choice

- `spawn_agent`: 独立した観点の subtask を走らせる
- `send_input`: 既存 agent に brief を追加する
- `wait_agent`: 結果が次ステップの前提になるときだけ使う

---

## M Patterns

### M-1: New Article With Light Research

- spawn `keyword-specialist`
- spawn `research-specialist`
- wait for both only when outline designに入る直前
- run `planner` or `outline-specialist`
- run `implementer` with single-file ownership on `articles/<slug>.md`
- run `reviewer` and optionally `quality-specialist`

使いどころ:

- テーマはあるが切り口や根拠がまだ弱い記事

### M-2: Existing Article Polish

- run `implementer` only
- run `reviewer`

使いどころ:

- 見出し整理、表現改善、結論の明確化など

### M-3: Pre-Publish Check

- run `quality-specialist`
- run `publish-specialist`
- if frontmatter fix is needed, hand off to `implementer`

使いどころ:

- `published: false` の記事を公開候補へ上げる直前

---

## L Patterns

### L-1: New Technical Article Plus Workflow Update

- spawn `keyword-specialist`
- spawn `research-specialist`
- coordinator synthesizes angle and missing facts
- run `planner` / `outline-specialist`
- run `implementer` on the article file
- run `docs-writer` on any changed checklist or workflow doc
- run `reviewer` and `quality-specialist`

使いどころ:

- 記事本文だけでなく、今後の運用ルールも一緒に変わるとき

### L-2: Repo-Local Skill Introduction

- run `planner`
- run `docs-writer` on `.codex/skills/`, `docs/requirements.md`, `docs/structure.md`
- run `implementer` only for any article-side examples if needed
- run `reviewer`

使いどころ:

- skill 本体と文書をまとめて追加するタスク

---

## X Patterns

### X-1: External Publish Adapter

- run `planner`
- run `publish-specialist`
- run `docs-writer`
- run `reviewer`

使いどころ:

- WordPress など Zenn 以外の公開先を扱う設計を追加するとき

注意:

- 認証方式、秘密情報配置、実行責務を決めるまでは実装に入らない

### X-2: Analytics Automation

- run `planner`
- run `analytics-specialist`
- run `docs-writer`
- run `reviewer`

使いどころ:

- Search Console やアクセス解析基盤を接続して定期分析を始めるとき

注意:

- 指標ソース未決のまま実装しない

---

## Coordinator Checklist

- delegation が明示要求か確認したか
- complexity を S / M / L / X で明示したか
- write scope が衝突していないか
- read-only specialist と worker を混同していないか
- `published` の変更を最終段階まで遅らせたか
- findings を `blocking` / `follow-up` / `accepted risk` に分類したか

---

## Do Not

- delegation 要求がない通常記事作業で reflex 的に subagent を使わない
- 同じ article ファイルを複数 worker に触らせない
- 未確認の事実を research 完了前に本文へ断定で書かない
- `publish-specialist` に外部投稿を暗黙に期待しない
- `analytics-specialist` に入力データなしで順位分析を断定させない
