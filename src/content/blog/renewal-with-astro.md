---
title: 'ブログを Astro でリニューアルしました'
description: '2012年から放置していた seckie.github.io を Astro 製のブログとして作り直しました。'
pubDate: 'Jul 05 2026'
tags: ['astro', 'blog']
---

長らく放置していた [seckie.github.io](https://seckie.github.io/) を、[Astro](https://astro.build/) 製の個人ブログとしてリニューアルしました。

## 構成

- **フレームワーク**: Astro(公式 blog テンプレートベース)
- **記事管理**: Markdown ファイルを GitHub リポジトリで管理
- **ホスティング**: GitHub Pages
- **デプロイ**: master ブランチへの push をトリガーに GitHub Actions で自動デプロイ

## 記事の書き方

`src/content/blog/` に Markdown ファイルを置くだけで記事になります。

```markdown
---
title: '記事タイトル'
description: '記事の説明'
pubDate: 'Jul 05 2026'
tags: ['tag1', 'tag2']
---

本文をここに書く。
```

ローカルでは `npm run dev` を実行すると、`http://localhost:4321` でプレビューできます。
ファイルを保存すると即座に反映されるので、Markdown を書きながらレンダリング結果を確認できます。

今後はエンジニアリングマネジメントや開発の話を書いていく予定です。
