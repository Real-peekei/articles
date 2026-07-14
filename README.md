# Quarto site scaffold — biostatistics/bioinformatics articles

## What's in here

```
_quarto.yml            site config: nav, theme (flatly), toc
index.qmd               listing page — auto-detects everything in articles/
about.qmd                short about page
styles.css               minor tweaks on top of flatly
articles/
  survival-analysis-final.qmd   your Cox PH article, converted to Quarto
.github/workflows/publish.yml   optional: auto-render + deploy on every push
```

## One-time setup

1. **Install Quarto** (if not already): https://quarto.org/docs/get-started/
   Check with `quarto --version` in your terminal.

2. **Put this folder's contents into your repo.**
   Either a brand-new repo (e.g. `articles` or `<username>.github.io`), or
   as a subfolder of `biostatistics-genomics-portfolio` — your call, but
   keep `_quarto.yml`, `index.qmd`, `about.qmd`, `styles.css`, and
   `articles/` at the same level (the project root).

3. **Check the R packages** used by your articles are installed locally
   (survival, survminer, tidyverse, broom, etc.) — Quarto calls your local
   R/knitr to render `.qmd` files with R chunks, same as RStudio does for `.Rmd`.

## Adding an article (this is the repeatable step, every time)

1. Upload the knitted `.Rmd` to me.
2. I convert the YAML header to Quarto format and drop it into `articles/`
   as a `.qmd` — same R code, same prose, just a header that adds:
   - `date:` — set this to when you want it to show as published
   - `description:` — one-line summary, shows on the listing card
   - `categories:` — tags like `[Biostatistics, Bayesian, RStats]`
3. You run:
   ```bash
   quarto render
   ```
   This re-executes every article's R code and rebuilds `index.qmd`'s
   listing automatically — you never touch the article list by hand.
4. Preview locally before publishing:
   ```bash
   quarto preview
   ```

## Publishing to GitHub Pages

### Option A — manual, one command (simplest)

```bash
quarto publish gh-pages
```

This renders the site and pushes it to a `gh-pages` branch, and sets up
GitHub Pages to serve from there automatically on first run. Repeat this
command any time you add or update an article.

### Option B — GitHub Actions, auto-rebuild on every push

The `.github/workflows/publish.yml` file in this scaffold does this for
you: push to `main`, GitHub spins up R + Quarto, re-renders everything
from source, and deploys to the `docs/` folder → GitHub Pages. Nothing to
run locally except `git push`. Trade-off: slightly more setup (R package
installs in the Action need to match what your articles actually use —
add to the `install.packages(...)` line in the workflow as your article
list grows), but the site always reflects the real source `.qmd`, never
a stale local knit.

To use this option instead of Option A:
1. In your repo settings → Pages → set source to "GitHub Actions"
   (not "Deploy from a branch").
2. Push to `main`. The workflow handles the rest.

## Repo settings (GitHub Pages)

Repo → Settings → Pages:
- **Option A** users: source = "Deploy from a branch", branch = `gh-pages`, folder = `/ (root)`
- **Option B** users: source = "GitHub Actions"

## Notes

- `execute: freeze: auto` in `_quarto.yml` means Quarto only re-executes
  R code for files that changed since the last render — keeps rebuild
  time down as the article count grows.
- The listing on `index.qmd` sorts newest-first by `date`. Change
  `sort: "date desc"` to `"title asc"` or similar if you want a different order.
