# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal site for https://seckie.github.io/ built with Astro 7, deployed as a fully static site to GitHub Pages. Top page is a profile, `/blog/` holds Markdown-managed posts. The README is written in Japanese; site UI text is Japanese (`lang="ja"`) but dates render in en-US format.

## Commands

```bash
npm run dev       # Dev server at http://localhost:4321, hot reload
npm run build     # Production build to ./dist/
npm run preview   # Serve the built dist/ locally
```

There is no test suite or linter. `npm run build` is the main verification step — it runs Astro's content-collection schema validation and type checks against the strict tsconfig.

Requires Node >= 22.12.0 (`engines` in package.json).

## Deployment

Push to `main` triggers `.github/workflows/deploy.yml`, which builds with `withastro/action` (Node 24) and deploys to GitHub Pages. PRs against `main` run the build job only, not the deploy.

## Architecture

**Content collections** are the core mechanism. `src/content.config.ts` defines a single `blog` collection: a glob loader over `src/content/blog/**/*.{md,mdx}` with a zod frontmatter schema — `title`, `description`, `pubDate` required; `updatedDate`, `heroImage`, `tags` optional. The Markdown filename becomes the post ID and URL slug (`my-post.md` → `/blog/my-post/`).

**Routing** (`src/pages/`):
- `blog/[...slug].astro` — `getStaticPaths()` from `getCollection('blog')`, renders each post inside the `BlogPost` layout
- `blog/index.astro` — post list
- `index.astro` (profile), `about.astro`
- `rss.xml.js` — RSS feed built from the same collection; sitemap comes from the `@astrojs/sitemap` integration

**Layout/components**: `src/layouts/BlogPost.astro` is the only layout; it receives the post's frontmatter as props and composes `BaseHead`, `Header`, `Footer` from `src/components/`. `Header` hides the site title on the home page (`is-home` class keyed off `Astro.url.pathname`).

**Site-wide constants** (`SITE_TITLE`, `SITE_DESCRIPTION`, `AUTHOR_NAME`, `GITHUB_URL`) live in `src/consts.ts`; the canonical site URL lives in `astro.config.mjs`.

**Images**: hero images go in `src/assets/` and are referenced by path in frontmatter; the schema's `image()` helper routes them through `astro:assets`/sharp for optimization.

**Styles**: global styles in `src/styles/global.css` (imported via `BaseHead`); everything else is scoped `<style>` blocks inside each `.astro` file. CSS custom properties (`--gray-dark`, `--box-shadow`, etc.) are defined in global.css.

## Conventions

- Indentation is tabs, single quotes in JS/TS (match existing files)
- TypeScript is strict (`astro/tsconfigs/strict` + `strictNullChecks`)
- Dates are formatted in en-US via the `FormattedDate` component
