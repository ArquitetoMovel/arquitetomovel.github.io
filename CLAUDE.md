# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal site and blog for Alexandre Danelon ("Arquiteto Movel"), built with Hugo and the PaperMod theme, hosted on GitHub Pages at `https://arquitetomovel.github.io/`. The site is bilingual (Portuguese default, English).

## Commands

```bash
# Start local dev server with live reload
hugo server -D

# Build the site
hugo --minify --baseURL "https://arquitetomovel.github.io/"

# Create a new post
hugo new posts/post-title.md
```

## Architecture

- **Theme**: PaperMod, installed as a git submodule at `themes/PaperMod/`. Do not modify theme files directly — override via `layouts/` or `params` in `hugo.toml`.
- **i18n**: Translation strings live in `i18n/pt-br.yaml` and `i18n/en.yaml`. These override PaperMod defaults.
- **Content**: All content lives in `content/`. Bilingual posts use `.md` (pt-br) and `.en.md` (English) file suffixes side-by-side. The about page follows the same pattern with `index.md` / `index.en.md`.
- **Layouts**: Custom layout overrides go in `layouts/`. Currently only `layouts/_default/terms.html` is overridden.
- **Static assets**: Images and favicon files go in `static/`. Profile image is `static/images/profile.jpg`.
- **Deploy**: Push to `main` triggers GitHub Actions (`.github/workflows/hugo-deploy.yml`) which builds with Hugo 0.147.5 extended and deploys to GitHub Pages.

## Content Conventions

- Default language is `pt-br`; English content uses `.en.md` suffix.
- Post front matter requires: `title`, `date`, `draft`, `tags`, `categories`, `description`.
- `buildDrafts = false` — posts with `draft: true` won't appear in production.
- Profile mode is enabled; homepage shows profile card with buttons linking to `/posts` and `/about`.

## Key Configuration (`hugo.toml`)

- `comments = true` is set but no comment provider is wired up yet.
- `googleAnalytics = ""` — analytics ID not configured.
- Profile image URL is `/images/ale64684296.jpeg` — update `params.profileMode.imageUrl` when changing the profile photo.
