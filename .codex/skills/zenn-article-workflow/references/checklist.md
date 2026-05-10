# Zenn Publish Checklist

## Filename

- slug は readable English `kebab-case` か
- 既存 filename と衝突していないか

## Frontmatter

- keys は `title` -> `emoji` -> `type` -> `topics` -> `published` の順か
- `type` は `tech` か `idea` か
- `topics` は記事内容に沿っているか
- 公開指示がない限り `published: false` か

## Structure

- 導入で背景やきっかけが伝わるか
- 見出しの流れで結論まで自然に読めるか
- 読者が知りたい論点に先回りできているか

## Evidence and Claims

- 強い主張に根拠があるか
- 不確かな点は断定していないか
- 公式情報や一次情報を優先しているか

## Markdown Quality

- 表やコードブロックは読みやすいか
- リンク切れや不要な生 URL がないか
- placeholder や書きかけが残っていないか

## Publish Gate

- この記事はまだ下書きなのか、公開候補なのかが明確か
- `published` を `true` にする前に最後のレビューが済んでいるか
- 公開後に直す予定の宿題があるなら、公開前に把握されているか
