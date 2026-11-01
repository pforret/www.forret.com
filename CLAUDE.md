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
- **`.gitignore` must never contain `site/`.** mkdox2 template regenerations add that line. New built files (CSS/JS bundles, fonts, new pages) are then silently left out of commits, and the live site breaks (it renders unstyled and new pages return 404). Check `.gitignore` after every mkdox2 upgrade.
- Public URL: `https://www.forret.com/`, which is also `site_url` in `zensical.toml`. The repo is `pforret/www.forret.com` and its fallback address is `https://pforret.github.io/www.forret.com/`.
- Custom domain, a one-time setup: DNS is at Gandi, with a `www` CNAME pointing to `pforret.github.io.`. Then run `gh api -X PUT repos/pforret/www.forret.com/pages -f cname=www.forret.com` and enforce HTTPS in the Pages settings. With deploys from Actions, a `CNAME` file in `site/` is ignored.

## Structure

- `zensical.toml`: the whole site config (TOML, under the `[project]` table, not `mkdocs.yml`). It holds the theme (`material`, a vintage editorial look via `docs/stylesheets/extra.css` with a light/dark toggle, Fraunces, Work Sans and JetBrains Mono), the enabled Markdown extensions (admonitions, tabs, details, Mermaid via superfences, emoji, snippets, task lists) and the plugins (`search`, `llmstxt`, `tags`, `glightbox`, `privacy`, `minify`, `social`, `callouts`, `blog`, `rss`), as well as `nav` and `exclude_docs`.
- `docs/`: the content root. Each top-level section in `nav` becomes a navigation tab (`navigation.tabs`).
- `docs/todo/`: internal todo tracking. It is listed in `exclude_docs` in `zensical.toml`, so it is never rendered into `site/`. Put any other private folder in that same list.
- `docs/about/`: `index.md` (bio and photo `img/peter.jpg`, the GitHub avatar), `cv.md` (experience, side projects, education) and `timeline.md` (a Markdown list inside `<div class="timeline" markdown>`, styled by the timeline block in `extra.css`). The content comes from https://cv.forret.com/ plus LinkedIn posts. **Privacy:** never publish the full birth date, family details or MBTI from the CV.
- `docs/portfolio/`: overview cards and flagship pages (DCProof, toolstud.io, bashew). `opensource.md` is **generated**, so don't edit it: run `scripts/portfolio_repos.sh` (needs `gh` and `jq`) to refresh it from GitHub (non-fork, non-archived repos with ≥ 5★). Theme groups are mapped in the script, and repos not in the map appear under "Other".
- Tab order comes from the `nav` list in `zensical.toml`; the awesome-pages plugin is not installed, so `.pages` files do nothing. A new page or section has to be added to `nav`, or it won't appear in the navigation.
- `scripts/`: helper scripts. This folder sits outside `docs/`, so nothing in it is published.
- `docs/blog/`: the blog plugin root. Posts go in `docs/blog/posts/YYYY-MM-slug.md` with `title`, `date` and `categories` frontmatter, and a `<!-- more -->` excerpt marker. Post URLs use the `yyyy/MM` date format, and the RSS feed is built from `blog/posts/`.
- `docs/overrides/`: theme overrides (`custom_dir`). `main.html` only extends `base.html` (no announcement bar). `partials/integrations/analytics/custom.html` loads Simple Analytics, which is cookieless (`extra.analytics.provider = "custom"`), so there is deliberately no cookie consent banner.
- `docs/stylesheets/extra.css`: the vintage editorial theme (inspired by designmd.app/library/vintage-editorial): cream, ink and warm-brown variables for the `default` and `slate` schemes, Work Sans UI chrome, a heading rule and dot, bordered cards and CTA buttons, an ink footer. The `--md-footer-*` variables must be set on `.md-footer`/`.md-footer-meta` themselves, because the theme's palette CSS overrides them at scheme level.
- `docs/icon/`: the favicon and logo assets that `zensical.toml` points to.
- `site/`: build output. Don't edit it by hand, regenerate it with `mkdox2 build`. It **is committed on purpose**: the HTML is built locally and GitHub serves `site/` as the live website. After any content change, run `mkdox2 build` and commit both `docs/` and `site/` together, so the published site never falls behind the sources.

## Hand-tuned settings in `zensical.toml`

mkdox2 upgrades can regenerate `zensical.toml`; one already dropped `exclude_docs`. Every block marked `# hand-tuned:` in the file must survive such a regeneration. Check this list after any upgrade:
- Identity: `site_name`, `site_description`, `site_author` and `copyright` (Peter Forret, © 2004–2026). There is deliberately **no** `repo_url`/`repo_name`, so the header shows no repo link.
- `nav` (tab order: Home, Portfolio, About, Blog; `tags.md` is deliberately left out of the menu) and `exclude_docs` (`/todo/`).
- `features`: portfolio-style. These are on: `navigation.indexes`, `navigation.top`, `navigation.footer` and `toc.follow`. These are deliberately off: `header.autohide`, `toc.integrate`, `navigation.integrate`, `content.action.copy` and `announce.dismiss`.
- `[[project.theme.palette]]`: `primary`/`accent = "custom"` with light and dark entries and a toggle. The real colours are in `docs/stylesheets/extra.css` (`extra_css`).
- `theme.font`: `text = "Fraunces"`, `code = "JetBrains Mono"`. Work Sans for the UI is imported in `extra.css`; the privacy plugin self-hosts all three.
- `[[project.extra.social]]`: LinkedIn (`/in/peterforret`), GitHub, the blog, X and YouTube.
- `[project.extra.analytics] provider = "custom"` with no `[project.extra.consent]` block.
- The `"Portfolio"` section under `[project.plugins.llmstxt.sections]`.

## Leftover scaffold content to replace

- `docs/blog/posts/2024-0*-post*.md` are lorem-ipsum placeholder posts (now marked as drafts).
- `docs/about/index.md` is still a placeholder. It is reserved for the bio, CV and career overview (a separate todo item).
