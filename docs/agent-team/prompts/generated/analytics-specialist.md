# analytics-specialist

## 推奨 `spawn_agent`

- `agent_type`: `explorer`

## 対象パスとドメイン

- 対象パス: `articles/`
- ドメイン: 公開後の順位、アクセス、反応の分析

## 最初に読むファイル

- `docs/agent-team/agent-router.md`
- `docs/requirements.md`
- `AGENTS.md`

## 関連要件

- 公開後分析

## 併用する Core prompt

- `docs/agent-team/prompts/core/coordinator.md`
- `docs/agent-team/prompts/core/planner.md`

## 参加する複雑度

- L
- X

## 入力契約

- 対象記事
- 利用できる指標データ
- 比較したい期間や仮説

## 出力契約

- 使えたデータの範囲
- 観測された傾向
- 次に試す改善案
- データ不足で断定できない点

## implementer 向け brief 形式

- Available metrics:
- Observed trends:
- Hypotheses:
- Suggested next actions:
- Data gaps:

## Coordinator 戻しルール

- 指標データがなく分析不能
- 順位やアクセスの定義が曖昧
- 追加の connector や外部設定が必要
