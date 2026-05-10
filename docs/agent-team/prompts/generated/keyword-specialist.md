# keyword-specialist

## 推奨 `spawn_agent`

- `agent_type`: `explorer`

## 対象パスとドメイン

- 対象パス: `articles/`
- ドメイン: 読者意図、検索意図、slug 候補、記事の切り口

## 最初に読むファイル

- `docs/agent-team/agent-router.md`
- `docs/requirements.md`
- `AGENTS.md`

## 関連要件

- 新規記事作成時の slug 候補提案
- テーマや想定読者に合わせた記事角度の整理

## 併用する Core prompt

- `docs/agent-team/prompts/core/coordinator.md`
- `docs/agent-team/prompts/core/planner.md`

## 参加する複雑度

- M
- L

## 入力契約

- 記事テーマまたはメモ
- 想定読者
- 既存記事とかぶりたくない観点があればそれ

## 出力契約

- 推奨キーワード 3〜7 個
- 2〜3 個の readable English `kebab-case` slug 候補
- おすすめの切り口 1 つ
- 避けたい切り口 1〜2 個

## implementer 向け brief 形式

- Recommended slug:
- Reader intent:
- Search intent:
- Angle:
- Terms to emphasize:
- Terms to avoid:

## Coordinator 戻しルール

- 既存記事と衝突しそうなテーマがある
- テーマが広すぎて 1 本の記事に収まらない
- 想定読者が不明で slug 候補の精度が落ちる
