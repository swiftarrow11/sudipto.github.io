---
title: "Building this blog: Hugo, GitHub Actions, and zero servers"
date: 2026-10-16
description: "How I set up a fast, free, static blog as plain Markdown in a Git repo — the stack and the workflow."
tags: ["hugo", "github-actions", "static-site", "tutorial"]
showToc: true
author: Sudipto Rokshit
---

I wanted a blog that felt like writing in a text file and nothing more. I was
inspired by James Montemagno's post about rebuilding his site as a static blog
— plain Markdown in a repo, built by [Hugo](https://gohugo.io/), verified and
published by GitHub Actions, served from GitHub Pages. No database, no CMS.

Here's the stack, start to finish.

## The stack

| Piece | Tool |
|---|---|
| Generator | Hugo (extended) |
| Theme | PaperMod |
| Content | Markdown files in Git |
| CI | GitHub Actions |
| Hosting | GitHub Pages (free) |

## The repo layout

```text
swiftarrow11.github.io/
├── content/
│   ├── posts/        # your blog posts, as .md files
│   ├── about.md
│   ├── archive.md
│   └── search.md
├── themes/PaperMod   # git submodule
├── hugo.toml         # site config
└── .github/workflows/deploy.yml
```

## The deploy workflow

The magic is a single Actions workflow that builds the site and hands the
output to GitHub Pages. The key parts are the two least obvious permissions:

```yaml
permissions:
  contents: read
  pages: write      # <-- lets Actions publish pages
  id-token: write   # <-- needed for the deploy action

steps:
  - uses: actions/checkout@v4
    with:
      submodules: recursive   # pulls the PaperMod theme
  - uses: peaceiris/actions-hugo@v3
    with:
      extended: true
  - run: hugo --minify
  - uses: actions/configure-pages@v5
  - uses: actions/upload-pages-artifact@v3
    with:
      path: public
  - uses: actions/deploy-pages@v4
```

Every push to `main` rebuilds the site and publishes it. To publish I don't
even click a button — it just happens.

## Why this works for me

- **It's versioned.** Every post has a Git history. I can revert a typo with
  `git revert`.
- **It's fast.** Hugo builds hundreds of posts in under a second.
- **It's free.** GitHub Pages hosting is free for public repos.
- **Dark mode for free.** The PaperMod theme follows your OS light/dark
  preference and adds a toggle.
- **No vendor lock-in.** It's just files. I can move hosting whenever I want.

That last point is the real "cool" for me: this blog is a folder of text files,
and everything else is automation around it.