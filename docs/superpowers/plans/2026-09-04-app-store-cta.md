# App Store CTA Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [x]`) syntax for tracking.

**Goal:** Make the official App Store badge the primary CTA on the strenoa.com hero and closing sections, with “Open app” secondary and Contact support removed from those rows.

**Architecture:** Static HTML/CSS only. Replace the primary `.btn-primary` “Open app” link with an `<a>` wrapping Apple’s official badge image; keep “Open app” as `.btn-secondary`. Add minimal CSS for badge alignment in `.cta-row`. No JS or build step.

**Tech Stack:** Static HTML, CSS design tokens in `assets/css/styles.css`, self-hosted brand assets, GitHub Pages.

## Global Constraints

- App Store URL: `https://apps.apple.com/us/app/strenoa/id6795763207`
- Open app URL: `https://app.strenoa.com`
- Badge must be Apple’s official artwork (no custom redraw/recolor)
- Badge path: `brand/badges/` (e.g. `brand/badges/app-store-badge-black.svg`)
- Accessible name: `Download on the App Store`
- Same-tab navigation (no `target="_blank"`)
- Do not change `404.html`, header/footer Support links, or feature imagery
- No automated test suite — verify with static server + link checks + visual review

## File structure

| File | Responsibility |
|------|----------------|
| `brand/badges/app-store-badge-black.svg` (or `.png`) | Official Apple badge asset |
| `index.html` | Hero + closing CTA markup |
| `assets/css/styles.css` | `.app-store-badge` sizing/alignment |

---

### Task 1: Add official App Store badge asset

**Files:**
- Create: `brand/badges/app-store-badge-black.svg` (prefer SVG; fall back to high-res PNG if SVG unavailable)
- Create: `brand/badges/README.md` only if needed to note Apple source — prefer omitting README if the filename is self-explanatory

**Interfaces:**
- Consumes: none
- Produces: Badge file at `brand/badges/app-store-badge-black.svg` (or `.png`) for use as `src` in Task 2

- [x] **Step 1: Create the badges directory**

```bash
mkdir -p /workspace/brand/badges
```

- [x] **Step 2: Obtain Apple’s official US English “Download on the App Store” badge**

Source: [App Store Marketing Resources and Identity Guidelines](https://developer.apple.com/app-store/marketing/guidelines/). Prefer the black badge with outline suitable for dark backgrounds.

If the full artwork kit requires interactive download, use Apple’s publicly served badge SVG/PNG from the marketing guidelines tooling (badge generator / assets CDN) that matches the official artwork. Do not hand-draw or approximate the logo/type.

Save as:
- `brand/badges/app-store-badge-black.svg` preferred, or
- `brand/badges/app-store-badge-black.png` if only raster is available

Verify the file is non-empty and opens as a valid image:

```bash
file brand/badges/app-store-badge-black.*
ls -la brand/badges/
```

Expected: `SVG` or `PNG` file with non-trivial size (not a 0-byte placeholder).

- [x] **Step 3: Commit**

```bash
git add brand/badges/
git commit -m "Add official App Store badge asset for marketing CTAs."
```

---

### Task 2: Wire hero and closing CTAs in `index.html`

**Files:**
- Modify: `index.html` (hero `.cta-row` ~lines 69–72; closing `.cta-row` ~lines 165–168)

**Interfaces:**
- Consumes: Badge asset path from Task 1 (`/brand/badges/app-store-badge-black.svg` or `.png`)
- Produces: Two identical CTA rows with App Store badge link + secondary Open app

- [x] **Step 1: Replace the hero CTA row**

Find:

```html
            <div class="cta-row">
              <a class="btn btn-primary focus-ring" href="https://app.strenoa.com">Open app</a>
              <a class="btn btn-secondary focus-ring" href="https://app.strenoa.com/support">Contact support</a>
            </div>
```

Replace with (adjust image `src` extension to match Task 1):

```html
            <div class="cta-row">
              <a
                class="app-store-badge focus-ring"
                href="https://apps.apple.com/us/app/strenoa/id6795763207"
              >
                <img
                  src="/brand/badges/app-store-badge-black.svg"
                  alt="Download on the App Store"
                  width="120"
                  height="40"
                  decoding="async"
                />
              </a>
              <a class="btn btn-secondary focus-ring" href="https://app.strenoa.com">Open app</a>
            </div>
```

Use real intrinsic `width`/`height` matching the badge aspect ratio once the asset is known (keep displayed height ~40px via CSS).

- [x] **Step 2: Replace the closing CTA row the same way**

Find the closing section’s `.cta-row` (same previous markup as the hero) and replace with the identical App Store + Open app markup from Step 1.

- [x] **Step 3: Sanity-check markup**

```bash
rg -n "Contact support|app-store-badge|Open app|apps\\.apple\\.com" index.html
```

Expected:
- No `Contact support` in `index.html`
- Two `app-store-badge` links to the App Store URL
- Two `Open app` links to `https://app.strenoa.com`
- Header/footer still contain Support links (`app.strenoa.com/support`)

- [x] **Step 4: Commit**

```bash
git add index.html
git commit -m "Use App Store badge as primary CTA; keep Open app secondary."
```

---

### Task 3: Style the badge in the CTA row

**Files:**
- Modify: `assets/css/styles.css` (near `.cta-row` / `.btn` rules, ~lines 230–320)

**Interfaces:**
- Consumes: `.app-store-badge` class from Task 2
- Produces: Aligned badge height with secondary button; focus ring compatible

- [x] **Step 1: Add `.app-store-badge` styles after `.cta-row`**

```css
.app-store-badge {
  display: inline-flex;
  align-items: center;
  line-height: 0;
  border-radius: 0.5rem;
  text-decoration: none;
}

.app-store-badge img {
  display: block;
  height: 2.5rem;
  width: auto;
}
```

Do not recolor, add drop shadows, or change badge aspect ratio beyond uniform scale via `height` + `width: auto`.

- [x] **Step 2: Confirm focus styles still apply**

Existing `.focus-ring:focus-visible` already targets `.focus-ring`. No change required unless the badge needs a slightly larger `border-radius` match — keep the shared rule.

- [x] **Step 3: Commit**

```bash
git add assets/css/styles.css
git commit -m "Align App Store badge with marketing CTA row."
```

---

### Task 4: Verify locally

**Files:**
- Test: served `index.html` via static server (no automated suite)

**Interfaces:**
- Consumes: Tasks 1–3 deliverables
- Produces: Verification evidence (curl/link checks + screenshots)

- [x] **Step 1: Start static server from repo root**

```bash
python3 -m http.server 8000
```

- [x] **Step 2: Confirm pages and assets load**

```bash
curl -sI http://127.0.0.1:8000/ | head -5
curl -sI http://127.0.0.1:8000/brand/badges/app-store-badge-black.svg | head -5
curl -s http://127.0.0.1:8000/ | rg -o 'href="https://apps\\.apple\\.com[^"]+"|href="https://app\\.strenoa\\.com"|app-store-badge|Contact support' 
```

Expected:
- `/` returns 200
- Badge asset returns 200
- App Store href appears twice; `Open app` href to `app.strenoa.com` appears in CTA rows; no `Contact support`

- [x] **Step 3: Visual check (narrow + wide)**

Open `http://localhost:8000/`, capture hero and closing CTA screenshots at ~390px and ~1280px widths. Confirm badge is legible on `#0c0c0c`, wraps cleanly with “Open app”, and Support remains in header/footer.

- [x] **Step 4: Push and update PR**

```bash
git push -u origin cursor/app-store-cta-dc3d
```

Update the existing PR description to reflect implementation (not docs-only).
