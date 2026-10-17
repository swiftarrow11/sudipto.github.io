# Sudipto Rakshit's personal blog

A fast, static, serverless personal blog. Plain Markdown in Git → built by
[Hugo](https://gohugo.io/) → published by GitHub Actions → served free on
[GitHub Pages](https://pages.github.com).

Inspired by James Montemagno's
[post about rebuilding his blog as a static site](https://montemagno.com/rebuilding-my-blog-as-a-static-site-with-github-copilot-cli/).

## Stack

| Layer     | Tool                          |
|-----------|-------------------------------|
| Generator | Hugo (extended)               |
| Theme     | PaperMod (git submodule)      |
| Content   | Markdown files (`content/`)   |
| CI/CD     | GitHub Actions (`.github/`)   |
| Hosting   | GitHub Pages                  |

## Running locally

```bash
git clone --recurse-submodules https://github.com/swiftarrow11/swiftarrow11.github.io.git
cd swiftarrow11.github.io
hugo serve
# → http://localhost:1313
```

## Adding a post

Drop a Markdown file in `content/posts/` with this front matter:

```markdown
---
title: "My post title"
date: 2026-10-16
description: "One-line summary."
tags: ["tutorial"]
showToc: true
---
```

Push to `main` and the GitHub Actions workflow builds and deploys automatically.

## Layout

- `content/posts/` — your posts
- `content/about.md`, `archive.md`, `search.md` — standalone pages
- `hugo.toml` — site config (menu, social icons, theme)
- `.github/workflows/deploy.yml` — Pages deploy pipeline