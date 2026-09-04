# App Store CTA on strenoa.com

**Date:** 2026-09-04  
**Status:** Approved for planning  
**Product:** strenoa.com marketing site (static GitHub Pages)

## Goal

Surface the newly released iOS app as the primary call to action on the marketing landing page, while keeping the web app available as a secondary path.

## Decisions

| Topic | Choice |
|-------|--------|
| Product paths | Both App Store and web app matter |
| Primary CTA | App Store download |
| Secondary CTA | Open app → `https://app.strenoa.com` |
| Contact support in CTA rows | Remove from hero and closing; keep Support in header and footer |
| Badge treatment | Official Apple “Download on the App Store” badge (not a custom site button) |
| Approach | Simple swap in hero + closing only |

## Design

### CTA structure

**Hero** and **closing** (`Start training with Strenoa`) each show:

1. Official App Store badge linking to `https://apps.apple.com/us/app/strenoa/id6795763207`
2. Secondary “Open app” button linking to `https://app.strenoa.com`

**Header / footer:** existing Support (and legal) links unchanged. No “Contact support” button in either CTA row.

**404 page:** unchanged (only “Back to home”).

### Badge asset

- Use Apple’s official badge artwork from [App Store Marketing Resources and Identity Guidelines](https://developer.apple.com/app-store/marketing/guidelines/).
- Prefer SVG (or high-resolution PNG if SVG is unavailable in the kit). US English localization.
- On the dark marketing background (`#0c0c0c`), use the official black badge with Apple’s outline rule, or the official dark-background variant if that asset reads more clearly. Do not redraw, recolor, stretch disproportionately, or restyle the badge.
- Host the file in-repo at `brand/badges/` (e.g. `brand/badges/app-store-badge-black.svg`), alongside other brand assets.
- Accessible name via `alt` (and/or `aria-label` on the link): `Download on the App Store`.
- Open in the same tab (default link behavior).
- Size the badge so its height roughly aligns with the existing `.btn` row (~40px / `2.75rem` min-height context); add a small CSS wrapper (e.g. `.app-store-badge`) for alignment in `.cta-row` without altering Apple’s artwork proportions beyond allowed scaling.

### Open app button

- Keep existing `.btn.btn-secondary` styling.
- Label remains `Open app`.
- No longer uses `.btn-primary` in these rows (primary visual weight is the badge).

## Files to change

| File | Change |
|------|--------|
| `index.html` | Update hero and closing `.cta-row` markup |
| `assets/css/styles.css` | Badge wrapper sizing/alignment |
| New badge asset | Official Apple file under `brand/badges/` |
| `404.html` | No change |

## Out of scope

- Platform / UA detection to swap CTAs
- Google Play / Android
- Hero copy, feature images, or closing headline changes
- Header/footer link set changes
- Analytics / attribution parameters on the store URL (unless added later intentionally)

## Acceptance criteria

- First viewport and closing CTA each show App Store badge (primary) + Open app (secondary).
- Contact support is not present in those CTA rows; Support remains reachable via header and footer.
- Badge is an official Apple asset, legible on the dark background, and links to the Strenoa App Store URL above.
- Open app still links to `https://app.strenoa.com`.
- Layout remains usable on mobile and desktop; badge and button wrap cleanly in `.cta-row`.
- No changes required to `404.html`.

## Testing

- Serve repo root with a static server and verify hero + closing CTAs visually on a narrow and wide viewport.
- Confirm both links resolve to the expected destinations.
- Confirm keyboard focus styles still work on the badge link and Open app button.
