# bashew

**Role:** Author · **Code:** [github.com/pforret/bashew](<https://github.com/pforret/bashew>) · ⭐ my most-starred GitHub project

bashew is a Bash script micro-framework. It takes you from a small stand-alone script to a complete project with CI/CD and testing. Most of my [open-source tools](<https://www.forret.com/portfolio/opensource/index.md>) are built on it.

## Three ways to start

```bash
bashew.sh script    # a new stand-alone script with option parsing and colour output
bashew.sh project   # a new script project in its own folder, with README
bashew init         # initialise a bashew-based repo you just cloned, with CI/CD
```

## Features

**Self-contained**

All code lives in a single file, with no external dependencies. It comes with `README.md`, `CHANGELOG.md` and other Markdown files. You only edit `Script:main()` and your own functions; the template code sits at the end of the script.

**Option parsing**

Flags, options and parameters are defined in one place. The usage text and short and long option parsing are generated from that definition.

**Function library**

`IO:` functions with smart colour handling (no colours when piped), `Str:` for string manipulation, `Os:` for checks such as required programs.

**Batteries included**

Reads multiple `.env` files, has `--quiet` and `--verbose` modes, temp and log folders with automatic cleanup, correct script-folder detection (resolves symbolic links), and GitHub CI/CD with shellcheck.
