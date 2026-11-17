# swisszhang.github.io

Personal site — a single-page resume built on GitHub Pages with
[jekyll-theme-minimal](https://github.com/pages-themes/minimal).

Live at <https://swisszhang.github.io>.

## Structure

```
_config.yml              site identity, contact details, plugins
_layouts/default.html    thin override of the theme layout (drops the download
                         buttons, adds the contact block and a clean footer)
_includes/contact.html   sidebar contact list
assets/css/style.scss    theme import + typography, resume blocks, print rules
assets/img/favicon.svg   JZ monogram, used as the browser tab icon
index.html               the resume itself
Gemfile / Gemfile.lock   pinned local build dependencies
```

Content lives in `index.html`; all site metadata (name, location, email,
LinkedIn, GitHub) lives in `_config.yml`.

## Local preview

Yes — `bundle exec jekyll serve` is the one command. Full sequence from a fresh
clone:

```bash
git clone git@github.com:swisszhang/swisszhang.github.io.git
cd swisszhang.github.io

bundle install                 # once, and again whenever Gemfile changes
bundle exec jekyll serve       # → http://127.0.0.1:4000
```

Open <http://127.0.0.1:4000>. `serve` watches the source and rebuilds on save;
refresh the browser to see the change. Stop it with `Ctrl+C`.

Always call it as `bundle exec jekyll serve`, not bare `jekyll serve` — the
`bundle exec` prefix pins the Jekyll version listed in `Gemfile.lock` instead of
whatever happens to be installed globally.

### Useful variants

| What you want | Command |
| --- | --- |
| Auto-refresh the browser on save | `bundle exec jekyll serve --livereload` |
| Free port 4000 is taken | `bundle exec jekyll serve --port 4001` |
| Preview from your phone on the same Wi-Fi | `bundle exec jekyll serve --host 0.0.0.0` |
| Build only, no server | `bundle exec jekyll build` → writes `_site/` |
| Build the way GitHub Pages will | `JEKYLL_ENV=production bundle exec jekyll build` |
| See the real error behind a build failure | add `--trace` |

### Requirements

- Ruby 3.x (`ruby -v`). macOS ships Ruby 2.6 at `/usr/bin/ruby`, which Jekyll 4
  rejects — if `ruby -v` shows 2.6, install a newer one via Homebrew, `rbenv`, or
  `rvm` before running `bundle install`.
- Bundler (`bundler -v`). If missing: `gem install bundler`.
- Nothing else. No Node, no theme install — the theme comes from the gem.

### Troubleshooting

- **`Address already in use`** — another server holds port 4000, or a previous
  `serve` is still running: `lsof -i :4000` to find it, or just pass `--port 4001`.
- **`bundle: command not found`** — `gem install bundler`, then retry.
- **`Could not find gem ... in locally installed gems`** — the lockfile and the
  installed gems drifted apart: run `bundle install` again.
- **First `bundle install` is slow** — it compiles/downloads native extensions
  (`sass-embedded`, `nokogiri`). One-time cost; later runs are instant.
- **Page looks unstyled** — open it through the server, not as a `file://` path.
  The CSS link is root-absolute (`/assets/css/style.css`) and only resolves when
  served.
- **A wall of `Deprecation Warning [import]` on every build** — expected. The
  theme's own `_sass` files still use the old `@import` syntax, and the warning
  is emitted on each compile. It is cosmetic; the CSS builds correctly. It is
  deliberately *not* silenced in `_config.yml` via `sass.silence_deprecations`,
  because GitHub Pages may run an older Sass converter that rejects the option —
  a failed deploy is a worse trade than noisy logs.

`_site/` is generated output. Never edit it — every build overwrites it, and it
is gitignored.

## Publishing

GitHub Pages builds the `main` branch on every push — no CI config needed, no
`gh-pages` branch, no Actions workflow.

```bash
git add -A
git commit -m "Update resume"
git push origin main
```

The site goes live a minute or two after the push. Build status and the deploy
log are under **Settings → Pages** in the repository.

## Printing

`Cmd/Ctrl + P` renders a print-optimised version: the sidebar collapses into a
header block, links lose their colour, and section rules stay intact. "Save as
PDF" in the print dialog produces a clean two-page resume.
