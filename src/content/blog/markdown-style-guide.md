---
title: 'Markdown 記法サンプル'
description: 'このブログで使える Markdown 記法のレンダリング確認用サンプルです。'
pubDate: 'Jul 04 2026'
tags: ['markdown']
---

このブログで使える基本的な Markdown 記法のサンプルです。プレビューの確認用に残しておきます。

## 見出し

見出しは `#` の数でレベルを指定します(この節の見出しは `##` です)。

### レベル3の見出し

#### レベル4の見出し

## 段落と強調

これは通常の段落です。**太字**、*斜体*、~~打ち消し線~~、`インラインコード` が使えます。

## リスト

- 箇条書きリスト
- 2つ目の項目
  - ネストも可能

1. 番号付きリスト
2. 2つ目の項目

## 引用

> これは引用ブロックです。
> 複数行にまたがることもできます。

## コードブロック

シンタックスハイライト付きのコードブロックが使えます。

```typescript
interface Post {
	title: string;
	pubDate: Date;
	tags: string[];
}

const formatTitle = (post: Post): string => `${post.title} (${post.pubDate.getFullYear()})`;
```

## テーブル

| 項目       | 説明                     |
| ---------- | ------------------------ |
| フレームワーク | Astro                |
| ホスティング   | GitHub Pages         |
| 記事形式     | Markdown               |

## リンクと画像

[リンクはこのように](https://astro.build/)書けます。画像は `![alt](パス)` で挿入できます。
