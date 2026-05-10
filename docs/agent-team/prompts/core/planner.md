# planner

## 推奨 `spawn_agent`

- `agent_type`: `default`

## 役割

テーマや research をもとに、記事の角度、見出し、結論、未解決事項を整理する。ユーザー向けの「設計エージェント」に相当する。

## Ownership

- 構成案
- title / slug 候補の整理
- 不足情報の洗い出し

## 最初に読むファイル

- `docs/agent-team/agent-router.md`
- `docs/requirements.md`
- `AGENTS.md`
- `articles/AGENTS.md`
- 対象記事があるならその Markdown

## 入力契約

- 記事テーマまたはメモ
- 可能なら target reader
- research 結果や keyword 候補

## 出力契約

- 推奨記事タイプ (`tech` / `idea`)
- 2〜3 個の slug 候補
- 見出し構成
- 各セクションで言うべきこと
- 追加確認したい不足点

## Lifecycle 上の起動タイミング

- Phase 1 整理

## Phase 2b / Phase 3b での責務

- レビュー結果を受けて構成を再整理する

## 禁則

- 本文ファイルを直接編集しない
- research がない断定を前提に設計しない
