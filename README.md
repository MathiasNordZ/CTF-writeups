# CTF-writeups

Obsidian vault of CTF writeups — published as a [GitHub Pages](https://pages.github.com/)
site with a minimal terminal-dark Jekyll theme.

## Enable the site (one-time)

1. Push this repo to GitHub (already at `MathiasNordZ/CTF-writeups`)
2. GitHub → **Settings → Pages** → Build and deployment → Source: **Deploy from a branch**
3. Branch: **`main`**, folder: **`/ (root)`** → Save

The site goes live at **https://mathiasnordz.github.io/CTF-writeups/** a minute later.
Every push (obsidian-git does this automatically) rebuilds the site.

## Adding a writeup

Copy **`TEMPLATE.md`** (top of this repo — visible in Obsidian too), place it in a
new folder for the challenge, and fill in the front matter:

```yaml
---
layout: writeup
title: Challenge Name
category: CTFkom 2026      # homepage group — new categories appear automatically
tags: [pwn]                # pwn | rev | forensics | network | crypto | web | misc
date: 2026-10-28
permalink: /CTFkom/281026/challenge-name/   # mirror the folder path (see note below)
description: One-liner used for meta tags.
---
```

**Note on `permalink`:** keep it identical to the folder path of the file — that
way relative image links (`![flag](flag.png)`) keep working. Tip: put a blank
line between a paragraph and a fenced code block that contains empty lines.

## Repo layout

```
_config.yml                 site config + homepage category order (category_order)
index.html                  homepage (terminal-style writeup index)
404.html                   terminal-style not-found page
_layouts/                  page templates
_includes/writeup-list.html writeup rows used on the homepage
assets/css/main.scss        the entire theme
assets/favicon.svg
```

Site plumbing is excluded from the build (`.obsidian`, this README, etc. —
see `exclude:` in `_config.yml`).

## Local preview (optional)

```sh
gem install jekyll kramdown-parser-gfm jekyll-sitemap
jekyll serve --safe
# open http://localhost:4000/CTF-writeups/
```
