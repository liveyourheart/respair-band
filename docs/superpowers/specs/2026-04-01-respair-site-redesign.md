# Respair Site Redesign — Design Spec

**Date:** 2026-04-01  
**Status:** Approved

## Overview

Respair is being pulled out of the Destin for Dirt website and launched as a standalone site at `respair.band`. The existing build has good bones — accurate shows, working platform links, functional contact form — but lacks any identity content. This redesign adds that content and restructures the page around a four-tab navigation.

---

## Page Structure

The page is a single HTML document. Two elements are always visible regardless of active tab:

1. **Logo** — the existing Rawb Carter lantern image, centered, 75% width (100% on mobile)
2. **Streaming links** — all five icons (Apple Music, Bandcamp, Instagram, Spotify, YouTube) in a row below the logo

Below those, a tab nav followed by the active tab's content:

```
LOGO
Streaming links
[ Band ]  [ Shows ]  [ Press ]  [ Contact ]
─────────────────────────────────────────
tab content
```

---

## Tab Navigation

**Implementation:** Vanilla JS (~20 lines), inline in `index.astro`. No framework.

- Tabs are anchor links: `<a href="#band">`, `<a href="#shows">`, etc.
- On page load, script reads `window.location.hash` and activates the matching tab. Default tab if no hash: **Band**.
- On tab click, script updates `window.location.hash` and shows/hides sections.
- Direct links work natively: `respair.band/#press`, `respair.band/#contact`, etc.

**Visual treatment:** Tab labels use JSL Blackletter (see Typography). Active tab: `#774F17`. Inactive: `#444`. No borders, no underlines — just color change.

---

## Tab Contents

### Band
- **Bio** — one paragraph, written by Sean and Richard. Placeholder text on launch if not yet delivered; drops in when ready.
- **Benefit line** — "Respair has performed benefit shows for The Homeless Alliance and Pivot OKC."
- **Band photo** — placeholder slot. Drops in when a photo is available.

### Shows
Unchanged from the current build. Existing current/past show split, existing linked show pages. No structural changes needed.

### Press
- **Pull quote** — "Everything you're missing in your collection, and your life… reminiscent of From Ashes Rise, Tragedy, Remains of the Day, and early Neurosis." Attributed to Keep Oklahoma Heavy with a link to their site.
- **Podcast mention** — placeholder until the podcast is identified.
- **EPK** — PDF hosted at `public/press/epk.pdf`. Rendered as "EPK — coming soon" (plain text, no link) until the file is dropped in and the link is activated in code.
- **One Sheet** — PDF hosted at `public/press/one-sheet.pdf`. Same approach.
- **Press email** — `amomentofrespair@gmail.com` as a visible `mailto:` link.

### Contact
- **Email** — `amomentofrespair@gmail.com` as a visible `mailto:` link, displayed above the form. For press/bookers who prefer direct email.
- **Contact form** — existing Web3Forms form, unchanged. Dev mode uses test key; production uses live key.

---

## Typography

**JSL Blackletter** applied to: tab nav labels, h1, h2 headings, and form submit button.

- Hosted in `public/fonts/` as `.woff2` (and `.ttf` fallback)
- Declared via `@font-face` in `respair.css`
- Fallback: `'Arial Black', sans-serif`
- Font file to be dropped in after launch — Arial Black used as placeholder in the interim

---

## Infrastructure

### In this repo
- Add `.gitignore` covering `node_modules/`, `dist/`, `.astro/`, `.superpowers/`
- Add `<meta name="description">` to all show pages (currently missing)
- Verify show pages link to `/` for the Respair band block (already correct — no `destinedfordirt.com` references in show page code)

### Out of scope (external)
- **301 redirect** — `destinedfordirt.com/respair/` → `respair.band` (DNS/server level, not in this repo)
- **Destin for Dirt hub** — update `destinedfordirt.com` to link to `respair.band` instead of the old path
- **Bandcamp tag audit** — review whether the "emo" tag is appropriate given the band's target audience

---

## Content Status at Launch

| Content | Status |
|---|---|
| Logo | Ready |
| Streaming links | Ready |
| Shows (all pages) | Ready |
| Contact form | Ready |
| Press quote | Ready |
| Bio | In progress — Sean + Richard |
| Band photo | TBD |
| Podcast mention | TBD — to be identified |
| EPK (PDF) | In progress |
| One Sheet (PDF) | In progress |
| JSL Blackletter font file | To be dropped in post-launch |

Items marked TBD or In Progress use placeholders at launch and are dropped in as they become available. The site is fully functional without them.
