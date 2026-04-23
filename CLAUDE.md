# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal portfolio site for Mukund Singh Bisht deployed via GitHub Pages. It is built on the HTML5 UP **Massively** template (CCA 3.0 — see `README.txt` and `LICENSE.txt`) and is essentially a static HTML/CSS/JS site with a minimal Jekyll wrapper.

## Build / run

- There is **no build pipeline in this repo**: no `package.json`, no `Gemfile`, no bundler, no SASS compiler wired up. GitHub Pages handles publishing.
- `_config.yml` sets `theme: minima` so GitHub Pages serves the repo as a Jekyll site, but all pages are plain HTML (no front matter, no layouts), so Jekyll mainly acts as a hosting shim.
- Local preview: open `index.html` directly, or run a throwaway static server (e.g. `python3 -m http.server`) from the repo root. To match GitHub Pages' Jekyll processing run `bundle exec jekyll serve` after installing `github-pages`, but this isn't required for the HTML/CSS/JS changes that make up normal edits here.
- No tests and no linters are configured.

## Architecture

Three top-level HTML pages, all stand-alone (no shared template / include system):

- `index.html` — the real portfolio. Contains the intro, nav, and the experience/project cards. **This is the file to edit for content changes** (jobs, projects, links, bio).
- `generic.html`, `elements.html` — untouched template reference pages from Massively (typography, buttons, forms, etc.). Useful as a style cheatsheet; not linked-to content.

Each page loads the same asset bundle in this order (see bottom of `index.html`):
`jquery.min.js` → `jquery.scrollex.min.js` → `jquery.scrolly.min.js` → `browser.min.js` → `breakpoints.min.js` → `util.js` → `main.js`.

### CSS pipeline (important)

The site ships **pre-compiled CSS** at `assets/css/main.css` and `assets/css/noscript.css`. The SASS sources under `assets/sass/` (organized into `base/`, `components/`, `layout/`, `libs/` and imported from `main.scss`/`noscript.scss`) are **not compiled by anything in this repo** — editing a `.scss` file has no effect on the served site unless you also regenerate the CSS.

Options when changing styles:
1. Edit `assets/css/main.css` directly (fastest for small tweaks, what the existing commits appear to do).
2. Edit the SASS, then recompile with an external sass tool (e.g. `sass assets/sass/main.scss assets/css/main.css`) and commit both.

Prefer option 1 for small fixes to avoid out-of-sync source/output; use option 2 only when you also intend to regenerate and commit the CSS.

### Breakpoints

Breakpoints are defined in **two** places that must stay in sync: `assets/sass/main.scss` (for the compiled CSS) and `assets/js/main.js` (for the JS `breakpoints()` helper that drives the nav panel, parallax, etc.). Named tiers: `default / xlarge / large / medium / small / xsmall / xxsmall`.

### JS behavior

`assets/js/main.js` wires up: a jQuery `_parallax` plugin for the background, a Scrollex-based intro fade, and a slide-out `#navPanel` for narrow viewports (generated at runtime from `#nav`). The other `jquery.*` files are vendored plugins from the template — don't modify.

## Conventions observed in existing edits

- Content (job titles, dates, project blurbs, links) lives inline in `index.html` as literal HTML — there is no data file or CMS.
- Images under `images/` are referenced by filename from `index.html`; add new project screenshots there.
- The project uses the upstream template's class names (`post featured`, `posts`, `image fit`, `actions`, `icon brands fa-*`, …). Reuse them rather than inventing new ones so the existing CSS applies.
