# sina.page

Personal site built with [Hugo](https://gohugo.io) and the [Congo](https://github.com/jpanther/congo) theme, deployed to GitHub Pages.

The toolchain is pinned end to end: the dev container and the GitHub Actions build use the same Hugo binary, and the theme is committed under `_vendor/`. Nothing upstream can change what your build produces — upgrades happen only when you make them.

## Preview locally

Open the folder in VS Code and choose **Reopen in Container**, then press <kbd>F5</kbd>. This starts the Hugo dev server and opens http://localhost:1313; the server stops when you end the debug session.

The dev container is optional. If you have Hugo extended 0.149.1 installed on your machine, everything works the same way on the host.

From a terminal:

```bash
hugo server              # preview at http://localhost:1313
hugo server -D           # include drafts
hugo server --disableFastRender   # full rebuild on change, if edits don't show
```

`baseURL` is overridden at runtime by `hugo server` and by the CI build flag, so the config file never needs editing to preview locally.

## Tasks

Run via <kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> → *Tasks: Run Task*:

| Task | Purpose |
| --- | --- |
| `hugo: server` | Dev server on port 1313 |
| `hugo: stop server` | Kill a running dev server |
| `hugo: build (production)` | Same command CI runs, output to `./public` |
| `theme: update Congo` | Pull a newer Congo release and re-vendor it |

## Layout

| Path | Contents |
| --- | --- |
| `content/` | Site content (Markdown). Images live beside their page and are referenced by filename. |
| `config/_default/` | Site config, params, menus |
| `layouts/` | Local overrides of theme templates |
| `assets/` | Custom CSS, icons, images |
| `_vendor/` | The Congo theme, committed on purpose (see below) |
| `.devcontainer/` | Pinned Hugo + Go toolchain; `versions.env` is the single source of truth |
| `public/`, `resources/_gen/` | Build artifacts — gitignored |

## Adding an image

Drop the file next to the page's Markdown and reference it by name:

```markdown
![Alt text](my-diagram.png)
```

Congo's image render hook resolves it as a page resource and emits a responsive `<picture>` with WebP variants. A Markdown title string becomes the caption. For more control use the shortcode:

```markdown
{{< figure src="my-diagram.png" alt="Alt text" caption="Caption" class="mx-auto my-4 rounded-md w-2/3" >}}
```

## Deployment

Pushing to `main` triggers [`.github/workflows/gh-pages.yml`](.github/workflows/gh-pages.yml), which publishes `./public` to the `gh-pages` branch.

CI does not build the dev container — that would cost minutes per run. Instead it downloads the *same* Hugo release, verified against the *same* checksum, read from the *same* [`.devcontainer/versions.env`](.devcontainer/versions.env) the container uses. The binary is byte-identical to the local one, so a successful local build means a successful deploy, and the whole install takes a few seconds.

Theme customisations must go in this repo's `layouts/` — edits inside `_vendor/` will be overwritten the next time the theme is updated.

## Why things are pinned

| Pinned thing | Where | What it prevents |
| --- | --- | --- |
| Hugo 0.149.1 extended, SHA256-verified | [`versions.env`](.devcontainer/versions.env) | A `brew upgrade` silently moving you to a Hugo with breaking template changes |
| Go 1.25.1, SHA256-verified | [`versions.env`](.devcontainer/versions.env) | Toolchain drift between machines |
| Congo v2.12.2 | `go.mod` + committed `_vendor/` | Builds needing the Go module proxy; a theme release changing your site |
| GitHub Actions, by commit SHA | [`gh-pages.yml`](.github/workflows/gh-pages.yml) | A moved upstream tag changing what runs in CI |

A production build has been verified to succeed with networking fully disabled, which confirms nothing is fetched at build time.

## Build speed

Measured on this site (19 pages, 33 processed images):

| | Time |
| --- | --- |
| First build after a change to images | ~4.9 s |
| Every subsequent build | ~80 ms |

Processed images are cached in `resources/_gen`, which lives on your disk and is bind-mounted into the container, so it survives container rebuilds. `hugo server` rebuilds incrementally on save. CI caches the same folder between runs.

Only the very first `Reopen in Container` is slow, while Docker pulls the base image. After that the image is cached and startup is a few seconds.

## Upgrading

Each upgrade is a deliberate, reviewable commit.

**Theme** — run the `theme: update Congo` task (or `hugo mod get -u github.com/jpanther/congo/v2 && hugo mod tidy && hugo mod vendor`), preview, then commit the `go.mod`, `go.sum` and `_vendor/` changes together.

**Hugo or Go** — edit [`.devcontainer/versions.env`](.devcontainer/versions.env), updating each version together with its checksums. Both the container and CI pick the change up automatically.

```bash
# Hugo checksums for the version you want
curl -fsSL https://github.com/gohugoio/hugo/releases/download/vX.Y.Z/hugo_X.Y.Z_checksums.txt \
  | grep -E 'hugo_extended_X.Y.Z_linux-(amd64|arm64).tar.gz'

# Go checksums
curl -fsSL 'https://go.dev/dl/?mode=json&include=all'
```

Then **Dev Containers: Rebuild Container** and run the `hugo: build (production)` task to confirm the site still builds.

## Prerequisites

Either Docker (for the dev container), or on the host: Hugo extended 0.149.1. Go is only needed if you update the theme.
