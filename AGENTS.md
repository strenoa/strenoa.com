# strenoa.com

The marketing site for `strenoa.com`, served as a static site on GitHub Pages. The custom domain is bound via the `CNAME` file, and `.nojekyll` disables Jekyll processing so files are served as-is.

## Structure

- `index.html` — landing page.
- `404.html` — not-found page (GitHub Pages serves this automatically).
- `assets/css/styles.css` — all styling and design tokens.
- `assets/fonts/` — self-hosted DM Sans (`.woff2`).
- `brand/` — logo (`brand/png/primary/white/...`) and favicons/manifest, mirroring the paths used by `app.strenoa.com`.

The design tokens (background `#050505`, text `#f2f2f2`, muted `#8a8a8a`, border `#222`, teal accent `#2dd4bf`, DM Sans, and the blue top radial glow) are copied from `app.strenoa.com` so the marketing site and the app stay visually consistent. CTAs/legal links point at `https://app.strenoa.com` (`/support`, `/privacy`, `/terms`).

## Cursor Cloud specific instructions

- There are no dependencies to install and nothing to build. The update script is intentionally a no-op.
- To preview locally, serve the repo root with any static server, e.g. `python3 -m http.server 8000` (Python 3 and Node 22 are preinstalled), then open `http://localhost:8000/`. Asset paths are root-absolute (`/assets/...`, `/brand/...`), so serve from the repo root, not a subdirectory.
- There are no lint or automated test suites configured.
