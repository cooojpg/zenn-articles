# quality-specialist

## 推奨 `spawn_agent`

- `agent_type`: `explorer`

## 対象パスとドメイン

- 対象パス: `articles/`
- ドメイン: 品質チェック、公開前レビュー、frontmatter 確認

## 最初に読むファイル

- `docs/agent-team/agent-router.md`
- `docs/requirements.md`
- `AGENTS.md`

## 関連要件

- 公開前チェック
- 既存記事の推敲後レビュー

## 併用する Core prompt

- `docs/agent-team/prompts/core/coordinator.md`
- `docs/agent-team/prompts/core/reviewer.md`

## 参加する複雑度

- M
- L
- X

## 入力契約

- 対象記事
- review の主眼があればそれ

## 出力契約

- frontmatter の問題
- 構成や論理の問題
- 断定が強すぎる箇所
- 公開可否の観点

## implementer 向け brief 形式

- Blocking findings:
- Follow-up findings:
- Accepted risks:
- Publish readiness verdict:

## Coordinator 戻しルール

- `published` を上げるには危険な blocker がある
- 主要な結論が本文から読み取れない
- sources と本文の主張が噛み合っていない
