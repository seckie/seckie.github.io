# seckie.github.io

[Astro](https://astro.build/) 製の個人サイト。 https://seckie.github.io/

- トップページ: プロフィール
- `/blog/`: ブログ記事一覧(記事は Markdown で管理)

## セットアップ

```bash
npm install
```

## コマンド

| コマンド          | 説明                                             |
| ----------------- | ------------------------------------------------ |
| `npm run dev`     | ローカルプレビュー(`http://localhost:4321`)。保存で即時反映 |
| `npm run build`   | 本番ビルド(`./dist/` に出力)                   |
| `npm run preview` | ビルド結果をローカルで確認                       |

## 記事の書き方

`src/content/blog/` に Markdown ファイル(`.md`)を追加します。ファイル名がそのまま URL のスラッグになります(例: `my-post.md` → `/blog/my-post/`)。

```markdown
---
title: '記事タイトル'
description: '記事の説明(メタ情報・OGP に使用)'
pubDate: 'Jul 05 2026'
tags: ['tag1', 'tag2']
---

本文をここに Markdown で書く。
```

frontmatter のフィールド:

| フィールド    | 必須 | 説明                          |
| ------------- | ---- | ----------------------------- |
| `title`       | ✔   | 記事タイトル                  |
| `description` | ✔   | 記事の説明                    |
| `pubDate`     | ✔   | 公開日                        |
| `updatedDate` |      | 最終更新日                    |
| `tags`        |      | タグ(文字列の配列)          |
| `heroImage`   |      | ヒーロー画像(`src/assets/` 内のパス) |

## デプロイ

`master` ブランチに push すると、GitHub Actions(`.github/workflows/deploy.yml`)が自動でビルドして GitHub Pages にデプロイします。
