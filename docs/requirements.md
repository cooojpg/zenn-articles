# zenn-articles

<!--
生成元: requirements-builder skill for Codex
生成日: 2026-05-10
モード: Hybrid
-->

## 概要

このリポジトリは、Zenn に投稿する記事コンテンツと、その記事作成を支援する repo-local な Codex ワークフローを管理するためのコンテンツ制作リポジトリである。`articles/` 配下の Markdown を中心に、下書き作成、推敲、公開前チェック、必要時のみ使う agent-team を文書化して運用する。

<!-- fidelity: high, source: README.md + articles/*.md + user conversation on 2026-05-09/10 -->

---

## プロジェクト種別

その他（コンテンツ制作 / 出版ワークフロー管理リポジトリ）

<!-- fidelity: high, source: README.md + repository contents -->

---

## ターゲットユーザー / ステークホルダー

- 主利用者は、このリポジトリの所有者である単一の執筆者
- 副次利用者は、この repo 上で記事制作を補助する Codex
- 最終的な読者は Zenn 上で記事を読むエンジニア、とくに AWS や実務知見に関心がある読者

<!-- fidelity: high, source: articles/*.md + user conversation -->

---

## 機能要件

- `articles/` 配下で Zenn 記事の Markdown ソースを管理する
- `articles/` と `books/` には、Zenn が解釈する有効な記事・本・アセット以外の Markdown ファイルを置かない
- 新規記事作成時は、テーマやメモを受け取り、必要なら短くヒアリングしてから構成と本文を作る
- 新規記事作成時は、未指定なら 2〜3 個の readable な英語 `kebab-case` slug 候補を提案する
- 新規記事は `articles/<slug>.md` に保存し、frontmatter は `title`、`emoji`、`type`、`topics`、`published` を基本形とする
- 新規記事は原則 `published: false` で作成し、明示指示があるときだけ公開状態へ進める
- 既存記事の推敲依頼では、対象ファイルを直接編集し、変更点を要約して返す
- 公開前チェックでは、title、emoji、topics、見出しの流れ、リンク、コードブロック、主張の確度、`published` 状態を確認する
- Zenn 記事作成を支援する repo-local skill を `.codex/skills/` 配下に置ける構造にする
- ユーザーが明示的に delegation を要求した場合に限り、KW選定、リサーチ、設計、執筆、品質、公開、分析の agent-team を使えるようにする
- 現時点の `公開エージェント` は Zenn 向けの公開準備と `published` 切替判断を担い、WordPress 等への自動投稿は将来拡張とする

<!-- fidelity: high, source: user conversation on 2026-05-09/10 -->

---

## 非機能要件

> 各項目は必ず `該当` または `該当なし` が判断できる書き方にする。空欄や `TBD` だけで終わらせない。

### セキュリティ

該当。リポジトリ内に公開投稿用の資格情報や外部 CMS の認証情報は保存しない。記事内の事実確認では一次情報または公式情報を優先し、未確認の断定を避ける。agent-team 文書では、自動的な外部投稿や秘密情報の読み出しを前提にしない。

<!-- fidelity: high, source: user conversation + repo-local workflow design -->

### テスト・検証

該当。自動テスト基盤は現時点で前提にしないが、記事変更ごとに frontmatter の整合性、Markdown の読みやすさ、リンクやコードブロックの妥当性を手動確認する。公開前は品質レビューを必須の最終確認として扱う。

<!-- fidelity: high, source: user conversation + existing article structure -->

### デプロイ・運用

該当。実行系アプリのデプロイはないが、リポジトリは Git 管理され、Zenn 連携を前提に記事ファイルを運用する。Codex 用の repo-local 設定は `.codex/config.toml` に保持し、agent-team のスレッド数制限もここに合わせる。

<!-- fidelity: high, source: repository contents + repo-local workflow design -->

### ドキュメント整備

該当。`docs/requirements.md`、`docs/structure.md`、`AGENTS.md`、`docs/agent-team/` を整備し、記事作成や delegation の運用ルールを明文化する。記事文体や公開前チェックの詳細は repo-local skill の references へ切り出せる形にする。

<!-- fidelity: high, source: user conversation + planned repo scaffold -->

### データ・永続化

該当。永続データは Markdown ファイル、画像などのファイル資産、各種ドキュメントに限定する。DB やマイグレーション管理は不要で、記事コンテンツは `articles/`、将来の本コンテンツは `books/` に保存する。

<!-- fidelity: high, source: repository contents -->

### 外部連携

該当。Zenn の記事 frontmatter とリポジトリ運用に依存する。必要に応じて Web 検索や公式ドキュメント参照を使うが、WordPress 等の外部 CMS 投稿や Search Console / Analytics 連携は現時点では未設定で、将来拡張扱いとする。

<!-- fidelity: high, source: user conversation + repository contents -->

---

## 想定変更規模 / スコープ

今後は記事本数の増加に加えて、repo-local skill と agent-team 文書の改善が継続的に発生する想定である。コードベースというよりはワークフロー文書中心だが、記事作成フローの変更は複数ドキュメントに波及する。

- 機能追加の頻度: 中
- 設計変更の見込み: あり
- 想定される変更単位: 数ファイルから領域横断

<!-- fidelity: high, source: user conversation -->

---

## 制約・トレードオフ

- 記事品質を優先し、全文自動生成より「ヒアリング -> slug 候補 -> 構成案 -> 本文」の段階的フローを採る
- 新規 slug は readable な英語 `kebab-case` を採用するが、既存のランダム風 slug は一括移行しない
- `articles/` や `books/` に repo 管理用の `AGENTS.md` を置かず、ルールは root `AGENTS.md` や `.codex/skills/` 側へ寄せる
- agent-team は役割分離を行うが、ユーザーが delegation を明示したときだけ使う
- `公開エージェント` は当面 Zenn 公開準備までに留め、外部投稿自動化は別責務に分ける

<!-- fidelity: high, source: user conversation -->

---

## スコープ外

- WordPress や他 CMS への自動投稿
- 資格情報を使う外部公開処理の常設実装
- Search Console やアクセス解析基盤が未設定の状態での自動分析
- 日本語タイトルの機械的なローマ字変換による slug 生成

<!-- fidelity: high, source: user conversation -->

---

## ディレクトリ構造案

- `.codex/`
  - `config.toml`
  - `skills/`
- `articles/`
- `books/`
- `docs/`
  - `requirements.md`
  - `structure.md`
  - `agent-team/`
- `tasks/`
- `AGENTS.md`
- `.gitattributes`

<!-- fidelity: high, source: planned repo scaffold -->

---

## 複雑度別タスク例 (S / M / L / X)

- **S (single file / typo / format)**: 既存記事 1 本のタイトルや topics を微修正する
- **M (2〜5 files / 1〜2 領域)**: 新規記事 1 本を作成し、関連する style / checklist 文書も更新する
- **L (6+ files / 3+ 領域 / 新機能)**: repo-local skill、agent-team 文書、記事運用ルールをまとめて整備する
- **X (横断ルール / 認証境界 / データ形式 / 大規模リファクタ)**: 外部 CMS 連携や公開後分析を自動化するために認証方針と運用フローを追加する

<!-- fidelity: high, source: user conversation + planned repo scaffold -->

---

## 未決事項

| 項目 | 暫定値 | 判断保留の理由 | 確定時期の目安 |
|---|---|---|---|
| 外部公開対象 | 当面は Zenn 公開準備のみ | WordPress 等への自動投稿は repo の現状とズレるため | 外部投稿を本当に必要とするとき |
| 分析データソース | 未設定 | Search Console / Zenn / GA などの入力元がまだ決まっていない | 分析運用を始めるとき |

---
