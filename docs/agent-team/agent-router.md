# Zenn Article Agent Router

このファイルは、このリポジトリにおける Codex の agent-team routing 入口である。

## Delegation Gate

- この運用は、ユーザーが明示的に delegation / subagents / parallel agents / agent team を要求したときだけ有効
- 明示要求がない通常作業では、メインの Codex がローカルで作業し、この文書は planning guidance としてだけ使う
- 複数 worker が同じファイルを編集する並列実行は禁止
- agent thread budget は `agents.max_threads = 12`、`agents.max_depth = 1`

---

## User-Facing Agent Names

| ユーザー向け名称 | Prompt | 推奨 `agent_type` | 主責務 | 書き込み有無 |
|---|---|---|---|---|
| KW選定エージェント | `docs/agent-team/prompts/generated/keyword-specialist.md` | `explorer` | 検索意図、読者意図、slug 候補の整理 | read-only |
| リサーチエージェント | `docs/agent-team/prompts/generated/research-specialist.md` | `explorer` | 一次情報と競合観点の収集 | read-only |
| 設計エージェント | `docs/agent-team/prompts/generated/outline-specialist.md` + `docs/agent-team/prompts/core/planner.md` | `default` | 記事構成、見出し、論点順の設計 | read-only |
| 執筆エージェント | `docs/agent-team/prompts/core/implementer.md` | `worker` | `articles/*.md` の本文作成・修正 | write |
| 品質エージェント | `docs/agent-team/prompts/generated/quality-specialist.md` + `docs/agent-team/prompts/core/reviewer.md` | `explorer` | 品質レビュー、公開前チェック | read-only |
| 公開エージェント | `docs/agent-team/prompts/generated/publish-specialist.md` + 必要時 `implementer.md` | `explorer` / `worker` | 公開準備、`published` 切替判断、必要時のみ frontmatter 更新 | read-only / write |
| 分析エージェント | `docs/agent-team/prompts/generated/analytics-specialist.md` | `explorer` | 公開後指標の分析 | read-only |

注記:

- `公開エージェント` は現時点では Zenn 向けの公開準備が責務であり、WordPress など外部 CMS への自動投稿は含まない
- `分析エージェント` は、ユーザーが指標データを与えるか、将来 connector を整えたときに使う

---

## Core Prompt Inventory

| Core role | Prompt path | 推奨 `agent_type` | 主な使用複雑度 |
|---|---|---|---|
| coordinator | `docs/agent-team/prompts/core/coordinator.md` | `default` | M / L / X |
| planner | `docs/agent-team/prompts/core/planner.md` | `default` | M / L |
| implementer | `docs/agent-team/prompts/core/implementer.md` | `worker` | S / M / L |
| reviewer | `docs/agent-team/prompts/core/reviewer.md` | `explorer` | M / L / X |
| docs-writer | `docs/agent-team/prompts/core/docs-writer.md` | `worker` | M / L / X |

省略した Core role:

| Role | 省略理由 | Fallback |
|---|---|---|
| tester | 自動テスト基盤がなく、品質確認は reviewer / quality-specialist の手動レビューで担保するため | reviewer |
| security | 認証境界や secret handling の常設実装が現時点で存在しないため | coordinator |
| devops | 実行アプリのデプロイ基盤がなく、公開は主にコンテンツ運用だから | coordinator |

---

## Specialist Prompt Inventory

| Specialist | Prompt path | 推奨 `agent_type` | 対象ドメイン |
|---|---|---|---|
| keyword-specialist | `docs/agent-team/prompts/generated/keyword-specialist.md` | `explorer` | 読者意図、検索意図、slug 候補 |
| research-specialist | `docs/agent-team/prompts/generated/research-specialist.md` | `explorer` | 公式情報、競合視点、主張の裏取り |
| outline-specialist | `docs/agent-team/prompts/generated/outline-specialist.md` | `default` | 見出し設計、構成、タイトル候補 |
| quality-specialist | `docs/agent-team/prompts/generated/quality-specialist.md` | `explorer` | 品質レビュー、公開前確認 |
| publish-specialist | `docs/agent-team/prompts/generated/publish-specialist.md` | `explorer` | 公開準備、frontmatter readiness |
| analytics-specialist | `docs/agent-team/prompts/generated/analytics-specialist.md` | `explorer` | 公開後分析 |

---

## Standard Agent Lifecycle

1. 整理
2. 作業
3. レビュー / 検証
4. 完了処理

起動タイミング:

- `coordinator` は整理 phase の冒頭で起動し、必要な agent だけ選ぶ
- `planner` と read-only specialist は整理 phase で brief を作る
- `implementer` と `docs-writer` は write scope が確定してから作業 phase で起動する
- `reviewer` と `quality-specialist` は作業後に最終確認として起動する
- blocking finding が出た場合は Phase 2b 修正作業 -> Phase 3b 再レビュー / 再検証を回す

---

## Complexity Routing

| 複雑度 | 判定基準 | 代表例 |
|---|---|---|
| S | 1 ファイル変更 / 軽微修正 / 既存記事の表現修正 | 1 本の記事の title や topics を直す |
| M | 2〜5 ファイル変更 / 1〜2 領域 | 新規記事 1 本の作成、または記事 + checklist 更新 |
| L | 6 ファイル以上 / 3 領域以上 / 新ワークフロー | skill と docs と記事運用ルールをまとめて更新 |
| X | 外部連携・認証境界・横断ルール変更 | 外部 CMS 投稿や分析自動化を導入する |

プロジェクト固有例:

- S: `articles/<slug>.md` の推敲だけを行う
- M: `articles/<slug>.md` と `docs/agent-team/` のチェック項目を更新する
- L: repo-local skill、agent-team 文書、`AGENTS.md`、`requirements.md` をまとめて更新する
- X: Zenn 以外への公開先や分析データソースを追加し、運用方針を横断的に変える

---

## Task Routing

| タスク種別 | 推奨パターン | 補足 |
|---|---|---|
| 新規記事作成 | `keyword-specialist` + `research-specialist` を必要なら並列 -> `planner` / `outline-specialist` -> `implementer` -> `reviewer` / `quality-specialist` | ユーザーが delegation を要求した場合だけ並列化する |
| 既存記事推敲 | `planner` 任意 -> `implementer` -> `reviewer` | 小規模なら単独作業で十分 |
| 公開前確認 | `quality-specialist` -> `publish-specialist` -> 必要時 `implementer` | `published` を切り替えるのは最終確認後 |
| 公開後分析 | `analytics-specialist` -> `planner` | 入力データ不足なら不足点だけ返す |
| ワークフロー文書更新 | `docs-writer` -> `reviewer` | agent-team 文書と `AGENTS.md` を同期する |

---

## Same-File Parallel Rule

- 同じ `articles/<slug>.md` を複数 worker で同時編集しない
- `implementer` が article 本文を書いている間は、他 agent は read-only で観点出しだけを行う
- `docs/agent-team/` と `articles/` のように write scope が分かれている場合だけ worker の並列を許可する

---

## Wait Strategy

- `wait_agent` は、次のローカル作業がその結果に依存しているときだけ使う
- `keyword-specialist` と `research-specialist` のような独立観点は並列化し、待機中に coordinator が統合枠組みを準備する
- 結果を取り込んだら不要な agent は close する

---

## Related Files

- `docs/agent-team/team-patterns.md`
- `docs/agent-team/README.md`
- `AGENTS.md`
- `docs/requirements.md`
- `docs/structure.md`
