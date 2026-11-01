# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

Personal website of **Peter Forret**, tech consultant and entrepreneur. Target content: a portfolio (projects and companies), a CV, and a career overview, plus a blog. It is a static site built with Zensical (the Material for MkDocs successor) through the `mkdox` wrapper (v2). There is no application code, only Markdown content and theme configuration.

## Commands

Use **`mkdox2`**, not `mkdox`. `mkdox2` is a bash wrapper (installed via basher) that runs Zensical in Docker (image `pforret/mkdox-zensical`). The older `mkdox` command runs MkDocs, which needs a `mkdocs.yml`, so it doesn't work in this repo.

- `mkdox2 serve`: local preview with live reload (`-P <port>` sets the port). The user often already has this running.
- `mkdox2 build`: generate the static site into `site/`. **Warning:** after building, this also runs `git add docs/ zensical.toml site/`, then `git commit` and `git push`. The `-G` flag doesn't turn this off, because `GITPUSH` defaults to `0` and the `-z` test in the script treats that as set.
- `mkdox2 post`: scaffold a new blog post
- `mkdox2 toc <folder> <file>`: generate a table of contents for a folder's Markdown files

To build without committing (for example to verify a change), run the image directly:

```bash
docker run --platform linux/amd64 --rm -v "$PWD":/docs pforret/mkdox-zensical build
```

There are no tests or linters. A change is verified when the build finishes without problems and the pages in `site/` look right.

**Zensical version:** `exclude_docs` needs a recent Zensical (0.0.74 works, 0.0.24 ignored it). If excluded pages show up in `site/`, run `docker pull pforret/mkdox-zensical`.

## Publishing

The site is built locally, not in CI. `mkdox2 build` writes the HTML to `site/`, that folder is committed, and GitHub serves it as the website. There is no build step on GitHub.

- `.github/workflows/pages.yml` runs on every push to `main` that changes `site/**` (it can also be started by hand). It uploads `site/` unchanged to GitHub Pages. The Pages source is set to "GitHub Actions". Never add a build step there.
- Because `mkdox2 build` commits and pushes, **every build publishes the site**. For a test build that doesn't commit, use the plain `docker run ... build` command shown above.
- Pushing changes to `docs/` without rebuilding publishes nothing new. Rebuild first.
- Public URL: `https://www.forret.com/`, which is also `site_url` in `zensical.toml`. The repo is `pforret/www.forret.com` and its fallback address is `https://pforret.github.io/www.forret.com/`.
- Custom domain, a one-time setup: DNS is at Gandi, with a `www` CNAME pointing to `pforret.github.io.`. Then run `gh api -X PUT repos/pforret/www.forret.com/pages -f cname=www.forret.com` and enforce HTTPS in the Pages settings. With deploys from Actions, a `CNAME` file in `site/` is ignored.

## Structure

- `zensical.toml`: the whole site config (TOML, under the `[project]` table, not `mkdocs.yml`). It holds the theme (`material`, red palette, Nunito font), the enabled Markdown extensions (admonitions, tabs, details, Mermaid via superfences, emoji, snippets, task lists) and the plugins (`search`, `awesome-pages`, `blog`, `rss`).
- `docs/`: the content root. Each top-level folder becomes a navigation tab (`navigation.tabs`). Navigation is built from the folder layout. Because of `awesome-pages`, a `.pages` file in a folder can set the order and titles.
- `docs/todo/`: internal todo tracking. It is listed in `exclude_docs` in `zensical.toml`, so it is never rendered into `site/`. Put any other private folder in that same list.
- `docs/blog/`: the blog plugin root. Posts go in `docs/blog/posts/YYYY-MM-slug.md` with `title`, `date` and `categories` frontmatter, and a `<!-- more -->` excerpt marker. Post URLs use the `yyyy/MM` date format, and the RSS feed is built from `blog/posts/`.
- `docs/overrides/`: theme overrides (`custom_dir`). `main.html` sets the announcement bar, and `partials/integrations/analytics/custom.html` holds the custom analytics snippet (`extra.analytics.provider = "custom"`).
- `docs/icon/`: the favicon and logo assets that `zensical.toml` points to.
- `site/`: build output. Don't edit it by hand, regenerate it with `mkdox2 build`. It **is committed on purpose**: the HTML is built locally and GitHub serves `site/` as the live website. After any content change, run `mkdox2 build` and commit both `docs/` and `site/` together, so the published site never falls behind the sources.

## Leftover scaffold content to replace

The project was just scaffolded with `mkdox2 new`, and these placeholders still need replacing:
- In `zensical.toml`: `site_name`, `site_url` (`www.forret.test`), `site_description` and `copyright` are generic. The repo icon is set to Bitbucket.
- `docs/index.md` describes a different project (`oil-crisis-monitor`) and links to `about/features/*.md`, which doesn't exist.
- `docs/overrides/main.html`: the announcement still contains the literal `{SITE_NAME}`.
- `docs/blog/posts/2024-0*-post*.md` are lorem-ipsum placeholder posts.
- `docs/about/index.md` is empty. It is the natural home for the CV and career overview, or you can add sibling sections such as `docs/portfolio/` and `docs/cv/`.
