# gh-mine

> Glance at your open GitHub pull requests across all your repos.

A minimalist `gh` extension that prints a colored cross-repo table of your open
PRs and lets you pick one with `gum`. Designed for the "where do my PRs stand
right now?" use case — no full-screen TUI, no waiting.

## Why?

[`gh-dash`](https://github.com/dlvhdr/gh-dash) is a great full-screen TUI for
managing PRs and issues. `gh pr list` only works in the current repo.
`gh-mine` fills the gap: a fast colored cross-repo table you can glance at,
plus an optional `gum` picker — no full-screen TUI to wait for.

## Install

```sh
gh extension install olivierlemoal/gh-mine
```

## Usage

```sh
gh mine                       # interactive picker (default)
gh mine --table               # plain colored table (scriptable)
gh mine --owner ACME          # filter by repository owner
gh mine --limit 30            # max PRs to fetch
gh mine --no-color            # disable ANSI colors
gh mine --no-links            # disable OSC 8 hyperlinks
gh mine --version             # print version
```

In the default picker mode, type to filter, `Enter` opens the selected PR
in your browser, `Esc` quits.

In `--table` mode, the PR number is wrapped in an OSC 8 hyperlink — Ctrl+click
to open it in supported terminals (Ghostty, WezTerm, kitty, iTerm2, modern
GNOME Terminal, etc.).

## Requirements

- [`gh`](https://cli.github.com/) — authenticated (`gh auth login`)
- [`jq`](https://jqlang.github.io/jq/) — JSON parsing
- [`gum`](https://github.com/charmbracelet/gum) (optional) — for the
  interactive picker. If `gum` is not installed, `gh mine` falls back
  to `--table` mode automatically.

On macOS:

```sh
brew install jq gum
```

On Debian/Ubuntu:

```sh
sudo apt install jq
# gum: see https://github.com/charmbracelet/gum#installation
```

## Features

- **Fast** — single GraphQL query (PR data + CI status), no TUI to initialize.
- **Cross-repo** — shows all your open PRs across every repo you contribute to.
- **CI status** — at-a-glance check rollup state (success / failure / pending).
- **Color-coded age** — `fresh` (green), `old (>1mo)` (yellow),
  `⚠ stale (>6mo)` (red).
- **Draft markers** — drafts shown in gray with a `*` suffix.
- **Security highlight** — PRs with `fix(security)` or "security" in the title
  are flagged in red.
- **Clickable PR numbers** — OSC 8 hyperlinks on supported terminals.
- **Pipe-friendly** — auto-falls back to `--table` mode on non-TTY (no ANSI
  bytes leaking into your scripts).
- **Cross-platform date handling** — works on Linux (GNU date) and
  macOS (BSD date).

> **Note**: the CI column uses Nerd Font glyphs. If your terminal doesn't have
> a Nerd Font configured, you'll see boxes — install [a Nerd Font](https://www.nerdfonts.com/)
> or fall back to looking at age & security indicators.

## Examples

Filter by owner:

```sh
$ gh mine --owner my-org --table
Repo                  #       Title                                       CI  Updated     Age
---------------------------------------------------------------------------------------------------
my-org/api            #421    feat: add /healthz endpoint                 ✓   2026-04-28  fresh
my-org/web            #89     fix(security): sanitize user input          ✗   2026-03-12  old (>1mo)
my-org/legacy         #3      chore: bump deps                            ·   2025-08-04  ⚠ stale (>6mo)

3 open PR(s) (owner: my-org) — 1 stale ⚠
```

Pipe into another tool:

```sh
gh mine --table --no-color | grep "fix(security)"
```

## License

MIT — see [LICENSE](./LICENSE).
