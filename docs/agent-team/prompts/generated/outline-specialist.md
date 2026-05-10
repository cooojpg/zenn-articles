# outline-specialist

## 推奨 `spawn_agent`

- `agent_type`: `default`

## 対象パスとドメイン

- 対象パス: `articles/`
- ドメイン: 記事構成、見出し、タイトル候補、論点順

## 最初に読むファイル

- `docs/agent-team/agent-router.md`
- `docs/requirements.md`
- `AGENTS.md`
- `articles/AGENTS.md`

## 関連要件

- 記事構成・見出し作成
- `tech` / `idea` の型分け

## 併用する Core prompt

- `docs/agent-team/prompts/core/coordinator.md`
- `docs/agent-team/prompts/core/planner.md`

## 参加する複雑度

- M
- L

## 入力契約

- 記事テーマ
- keyword / research brief
- 想定記事タイプ

## 出力契約

- 推奨 article type
- タイトル候補 2〜3 個
- 見出し構成
- 各見出しで触れる要点
- 削るべき論点

## implementer 向け brief 形式

- Suggested title:
- Article type:
- Headings:
- Key point per heading:
- Conclusion:
- Points to omit:

## Coordinator 戻しルール

- 1 本の記事として論点が多すぎる
- `tech` と `idea` のどちらで書くべきか不明
- research 不足で構成が不安定
