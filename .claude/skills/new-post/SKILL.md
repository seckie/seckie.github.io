---
name: new-post
description: Use this skill every time the user wants a new article/post for this blog, no matter how casually they phrase it. Trigger whenever they bring up a topic, ネタ, or テーマ they want written up — 記事書きたい, 記事にしたい, 記事作って, ポスト書いて, ブログに載せたい, 下書き作って — even if it's just "何か書く" plus a topic idea, even if the structure isn't decided, and even if they mention only a tag or theme. A stated topic + any intent to write/draft/post = use this skill. Do not create the Markdown file by hand instead of using this skill; it owns the full flow (drafted Japanese body in src/content/blog/, post/<slug> branch, draft PR). Do NOT use it for editing or fixing existing posts, changing site design/pages/components, debugging the site, or committing unrelated work.
---

# New Blog Post

Take a topic from the user and turn it into a reviewable draft: a new Markdown article on its own branch with a draft PR. The author finishes the writing on that branch, so the goal is a solid first draft plus a clean git setup — not a finished article.

## 1. Gather article info

Determine from the user's request (ask only for what's genuinely missing):

- **Topic** — what the article is about. If the user gave only a title-level idea, ask one round of questions for the concrete points they want covered (experiences, tools, conclusions). A body drafted from nothing but a title tends to be generic filler that the author deletes anyway.
- **Slug** — short English kebab-case, e.g. `claude-code-automation`. This becomes the filename and the URL (`/blog/<slug>/`), so keep it stable and meaningful. Check that `src/content/blog/<slug>.md` doesn't already exist.
- **Title / description** — in Japanese. `description` is 1–2 sentences; it appears in the blog list, RSS, and meta tags.
- **Tags** — a few lowercase English keywords (e.g. `['astro', 'blog']`).

## 2. Branch from latest main

The branch must contain only this article, so start clean:

- If the working tree has unrelated changes, stop and ask the user how to handle them.
- `git checkout main && git pull`, then `git checkout -b post/<slug>`.

## 3. Create the article file

Write `src/content/blog/<slug>.md`. Frontmatter uses single quotes and the `'MMM DD YYYY'` date format (dates render site-wide in en-US):

```markdown
---
title: '記事タイトル'
description: '記事の説明(1〜2文)。'
pubDate: 'Jul 06 2026'
tags: ['tag1', 'tag2']
---
```

`updatedDate` and `heroImage` (image path under `src/assets/`) are optional; omit them for a new post unless the user provides them.

**Body**: draft it in Japanese, です・ます調, matching the tone and structure of the existing posts in `src/content/blog/` (short intro paragraph, then `##` sections). Write substantive prose from what the user actually told you. Where a fact is unknown — numbers, dates, names, outcomes — leave it out or mark it with an inline `<!-- TODO: ... -->` rather than inventing it; a plausible-sounding fabrication is worse than a gap, because the author may not notice it before publishing.

## 4. Validate

Run `npm run build`. This is the repo's only verification step — it validates the frontmatter against the content-collection zod schema and type-checks. Fix any errors before committing.

(Sandbox note: if npm fails with a cache EPERM, prefix with `export npm_config_cache="$TMPDIR/npm-cache"`.)

## 5. Commit, push, and open a draft PR

- Commit with an English message matching the repo's style: `Add blog post: <slug>`
- `git push -u origin post/<slug>`
- Create a **draft** PR — merging to main deploys to GitHub Pages immediately, so the PR must not look mergeable while the article is half-written:

```bash
gh pr create --draft --base main --title "<記事タイトル>" --body "..."
```

PR body: one-line summary of the article, then a short checklist for the author:

```markdown
新しい記事の下書きです。

- [ ] 本文を仕上げる
- [ ] `npm run dev` でプレビュー確認 (http://localhost:4321/blog/<slug>/)
- [ ] Ready for review にしてマージ(マージすると公開されます)
```

Finish by reporting the PR URL and the local preview URL to the user.
