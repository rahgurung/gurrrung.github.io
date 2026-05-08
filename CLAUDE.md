# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run build    # hexo generate — builds site into ./public
npm run server   # hexo server — local dev server at http://localhost:4000
npm run clean    # hexo clean — clears ./public and db.json cache
```

There are no tests. There is no lint command.

## Deployment

Pushing to `main` triggers `.github/workflows/pages.yml`, which builds the site and pushes the output to the `gh-pages` branch. GitHub Pages then serves that branch at rahulgurung.com. Do not run `npm run deploy` (hexo deploy) locally — it does the same thing and will conflict with CI.

## Architecture

This is a [Hexo](https://hexo.io) static site. Content lives in `source/`, the theme lives in `themes/minima/` (custom, not a published package), and the built output goes to `public/`.

**Content types:**
- `source/_posts/*.md` — blog posts. Each post has a same-named asset folder (e.g. `Post-Title/`) for images, referenced via `{% asset_img filename.png "alt" %}`.
- `source/projects/index.ejs` — Projects page, written in EJS directly (not markdown).
- `source/experience/index.ejs` — Experience page, same approach.

**Creating a new post:**
```bash
npx hexo new post "My Post Title"
```
This creates `source/_posts/My-Post-Title.md` and an asset folder `source/_posts/My-Post-Title/` for images. The scaffold pre-fills frontmatter from `scaffolds/post.md`. Fill in `description`, `readtime`, `tags`, and add a `header.png` to the asset folder for the thumbnail.

**Post frontmatter fields:**
```yaml
title: 'Post Title'
date: 2022-05-26 20:29:51
description: Short description for SEO/preview
thumbnail: '/Post-Title/header.png'
readtime: 5   # minutes, displayed on card
tags:
  - Tag Name
```

**Theme (`themes/minima/`):**
- `layout/layout.ejs` — base shell wrapping all pages
- `layout/index.ejs`, `post.ejs`, `page.ejs`, `archive.ejs`, `tag.ejs` — page-specific templates
- `layout/partial/` — header, footer, pagination, analytics, comments
- `source/css/` — theme styles (Stylus)
- Theme config is in `themes/minima/_config.yml` — controls nav menu, social links, accent color (`tcolor`), max posts on index, Google Analytics ID

**Site config (`_config.yml`):**
- `theme: minima` points to the custom theme
- `post_asset_folder: true` enables per-post asset folders
- `syntax_highlighter: prismjs` (highlight.js is disabled)
- `deploy` block configures `hexo-deployer-git` — only used if running `hexo deploy` manually (CI handles deployment instead)
