# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Academic portfolio site for A.D (David) Le, built on the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme. Deployed to `https://duylebkHCM.github.io` via GitHub Pages (gh-pages branch).

## Source of Truth for Profile Content

Before making **any** update to the personal website content (about, CV, publications, news, projects, etc.), always first check `G:\My Drive\My Research Resource Centralize\PhD apply resource` for the newest version of the profile (CV, research statement, publications, bio). Sync the site with the latest information found there, and flag any discrepancies to the user.

## Build & Dev Commands

```bash
# Start dev server (Docker, recommended)
docker compose pull && docker compose up
# Site at http://localhost:8080

# Rebuild after dependency/Dockerfile changes
docker compose up --build

# Stop
docker compose down

# Format code (required before every commit)
npx prettier . --write
# First time: npm install --save-dev prettier @shopify/prettier-plugin-liquid

# Legacy local build (without Docker)
bundle install && bundle exec jekyll serve --port 4000
```

## Architecture

- **Jekyll 4.x** static site generator with Liquid templates, SCSS, and extensive plugins
- **Docker image**: `amirpourmand/al-folio:v0.16.3` — includes Ruby, ImageMagick, all gems
- **`_config.yml`**: Central config — site metadata, feature flags (`enable_*`), plugin settings, third-party library versions with SRI hashes
- **`_pages/`**: Static pages (about.md is the homepage via `permalink: /`)
- **`_posts/`**: Blog posts (`YYYY-MM-DD-title.md`)
- **`_bibliography/papers.bib`**: Publications managed by `jekyll-scholar` (custom BibTeX keywords: `pdf`, `code`, `preview`, `selected`, etc.)
- **`_data/`**: YAML data files (socials, coauthors, cv, citations, venues, repositories)
- **`_layouts/` + `_includes/`**: Liquid templates — layouts reference includes
- **`_sass/`**: SCSS stylesheets
- **`_scripts/`**: Client-side JavaScript
- **Collections**: `books`, `news`, `projects`, `teachings` (configured in `_config.yml`)

## CI/CD

- **`deploy.yml`**: Builds with `JEKYLL_ENV=production`, runs purgecss, deploys to gh-pages branch. Triggers on push/PR to main for site-related file changes.
- **`prettier.yml`**: Fails PRs if code isn't formatted. Run `npx prettier . --write` locally.
- **`axe.yml`**: Accessibility testing
- **`broken-links.yml` / `broken-links-site.yml`**: Link validation
- **`codeql.yml`**: Security scanning

## Critical Rules

- **`url` and `baseurl` must match**: For personal site, `url: https://username.github.io` with empty `baseurl`. For project site, add `baseurl: /repo-name/`.
- **YAML special characters**: Quote values containing `:`, `&`, `#` — e.g., `title: "My: Cool Site"`
- **jekyll-scholar config**: `scholar.last_name` and `scholar.first_name` in `_config.yml` control author highlighting in publications. Currently set to Einstein (template default) — update to match actual author.
- **ImageMagick required**: Responsive image generation is enabled (`imagemagick.enabled: true`). Docker handles this; local dev needs it installed.

## File-Type Specific Guides

See `.github/copilot-instructions.md` for build details and common pitfalls. Per-filetype instructions:

| File Type | Guide |
|---|---|
| Markdown (`_posts/`, `_pages/`) | `.github/instructions/markdown-content.instructions.md` |
| YAML (`_config.yml`, `_data/`) | `.github/instructions/yaml-configuration.instructions.md` |
| BibTeX (`_bibliography/`) | `.github/instructions/bibtex-bibliography.instructions.md` |
| Liquid (`_includes/`, `_layouts/`) | `.github/instructions/liquid-templates.instructions.md` |
| JavaScript (`_scripts/`) | `.github/instructions/javascript-scripts.instructions.md` |
