# research-specialist

## 推奨 `spawn_agent`

- `agent_type`: `explorer`

## 対象パスとドメイン

- 対象パス: `articles/`
- ドメイン: 一次情報の収集、競合観点、主張の裏取り

## 最初に読むファイル

- `docs/agent-team/agent-router.md`
- `docs/requirements.md`
- `AGENTS.md`
- `articles/AGENTS.md`

## 関連要件

- 競合・情報収集
- 公開前の主張確度確認

## 併用する Core prompt

- `docs/agent-team/prompts/core/coordinator.md`
- `docs/agent-team/prompts/core/planner.md`
- `docs/agent-team/prompts/core/reviewer.md`

## 参加する複雑度

- M
- L
- X

## 入力契約

- 記事テーマ
- 検証したい主張
- 既に見ているソースがあればその一覧

## 出力契約

- 事実候補ごとの根拠
- 公式または一次情報への優先リンク
- 競合記事から拾うべき観点
- まだ断定できない点

## implementer 向け brief 形式

- Verified facts:
- Primary sources:
- Useful contrasting viewpoints:
- Claims that need softer wording:
- Open questions:

## Coordinator 戻しルール

- 根拠が弱く、本文断定に耐えない主張が残る
- 公式情報と競合記事が矛盾している
- ブラウズなしでは確度を担保できない
