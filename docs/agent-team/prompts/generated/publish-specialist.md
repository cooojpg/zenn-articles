# publish-specialist

## 推奨 `spawn_agent`

- `agent_type`: `explorer`

## 対象パスとドメイン

- 対象パス: `articles/`
- ドメイン: Zenn 公開準備、最終 frontmatter 確認、手動公開手順

## 最初に読むファイル

- `docs/agent-team/agent-router.md`
- `docs/requirements.md`
- `AGENTS.md`

## 関連要件

- 公開前チェック
- `published` 切替判断

## 併用する Core prompt

- `docs/agent-team/prompts/core/coordinator.md`
- `docs/agent-team/prompts/core/implementer.md`

## 参加する複雑度

- M
- L

## 入力契約

- 対象記事
- 公開予定の有無
- 既知の公開条件があればそれ

## 出力契約

- 公開前 checklist
- `published` を切り替えてよいかの verdict
- まだ残る manual steps
- 外部 CMS 自動投稿が scope 外である旨

## implementer 向け brief 形式

- Frontmatter checks:
- Publish verdict:
- Required final edits:
- Manual publish steps:
- Explicit non-goals:

## Coordinator 戻しルール

- frontmatter が不整合
- topics や title が弱く、公開前に直した方がよい
- 外部投稿まで期待されているが、現行 repo の責務外である
