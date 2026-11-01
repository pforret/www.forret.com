# Gotchas

One line each, newest last. Start a line with `path/to/file:` and the session-start hook shows it when that file is in your diff.

- 2026-11-01 zensical.toml: no awesome-pages plugin since mkdox2 v2.2.1, so .pages files are ignored; the nav list must name every top-level section
- 2026-11-01 docs/**/*.md: front matter must be on line 1; anything before it (even an HTML comment) stops it being parsed
- 2026-11-01 docs/about/timeline.md: the bold year range is styled display:block (extra.css timeline block), so write '- **YYYY–YYYY** text' with no separator after the year
- 2026-11-01 docs/stylesheets/extra.css: the h2::before 8px dot reads like a list bullet before headings; it's hidden only when the h2 contains an icon (.twemoji)
- 2026-11-01 docs/stylesheets/extra.css: the theme palette CSS overrides --md-footer-* and --md-typeset-a-color per scheme; set them on .md-footer/.md-footer-meta themselves (and .md-footer-meta also has .md-typeset, so typeset link colours leak into the footer)
- 2026-11-01 site/**/*.html: the minify plugin drops attribute quotes (href=https://…), so greps like 'href='…'' or 'github.com/x'' fail; parse or grep without quotes
- 2026-11-01 mkdox2 build: always git-commits and pushes (broken -z GITPUSH check), so every build triggers the Pages deploy; use the plain docker build for test builds
