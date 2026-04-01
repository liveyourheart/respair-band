# Respair Site Redesign — Design Spec

**Date:** 2026-04-01  
**Status:** Implemented

## Overview

Respair is being pulled out of the Destin for Dirt website and launched as a standalone site at `respair.band`. This redesign adds identity content and restructures the page around a four-tab navigation.

---

## Page Structure

```
LOGO (85% width desktop, 100% mobile)
Streaming links (60% width, centered)
══════════════════════════════════════  (double gold rule)
[ Band ]  [ Shows ]  [ Press ]  [ Contact ]
══════════════════════════════════════  (double gold rule)
tab content
```

---

## Tab Navigation

**Implementation:** Vanilla JS, inline in `index.astro`. No framework.

- Tabs are anchor links: `<a href="#band">`, `<a href="#shows">`, etc.
- On page load, script reads `window.location.hash` and activates the matching tab. Default tab if no hash: **Band**.
- On tab click, script updates `window.location.hash` and shows/hides sections.
- Direct links work: `respair.band/#press`, `respair.band/#contact`, etc.

**Visual treatment:** JSL Blackletter font, 4em desktop / 1.6em mobile. Active tab: `#774F17`. Inactive: `#444`.

---

## Tab Contents

### Band
- **Bio** — one paragraph, written by Sean and Richard. Placeholder on launch; drops in when ready.
- **Benefit line** — "Respair has performed benefit shows for The Homeless Alliance and Pivot OKC."
- **Band photo** — placeholder slot. Drops in when available.

### Shows
- Current/past show split with linked show pages.
- 01/24/2026 show shown with strikethrough, noted as rescheduled due to weather.

### Press
- **Pull quote** — KOH quote linked to `https://keepoklahomaheavy.substack.com/p/respair-the-return-of-hope`
- **Podcast** — Killing Time Podcast by Kill Murray, linked to YouTube at `t=1712s`
- **EPK** — "EPK coming soon" placeholder until PDF dropped into `public/press/epk.pdf`
- **One Sheet** — "One Sheet coming soon" placeholder until PDF dropped into `public/press/one-sheet.pdf`
- **Contact link** — "Contact us" link that switches to the Contact tab (no email address in Press)

### Contact
- **Email** — `amomentofrespair@gmail.com` as a visible `mailto:` link above the form
- **Contact form** — Web3Forms. Dev mode uses test key; production requires a new key (see post-launch checklist)

---

## Typography

**JSL Blackletter** applied to: tab nav labels, h1, h2 headings, form submit button.

- Font file: `public/fonts/JBLACK.TTF`, with `JSLBlackletter.ttf` as secondary fallback
- Declared via `@font-face` in `respair.css`
- System fallback: `'Arial Black', sans-serif`

---

## Infrastructure

### In this repo
- `.gitignore` covering `node_modules/`, `dist/`, `.astro/`, `.superpowers/`
- `public/press/` directory reserved for EPK and one-sheet PDFs
- `public/fonts/` directory with `JBLACK.TTF`
- Meta descriptions on all six show pages

### Out of scope (external)
- **301 redirect** — `destinedfordirt.com/respair/` → `respair.band`
- **Destin for Dirt hub** — update link to `respair.band`
- **Bandcamp tag audit** — review "emo" tag

---

## Content Status at Launch

| Content | Status |
|---|---|
| Logo | Ready |
| Streaming links | Ready |
| Shows (all pages) | Ready |
| Contact form | Ready (needs new Web3Forms key before launch) |
| Press quote + KOH link | Ready |
| Podcast mention | Ready — Killing Time by Kill Murray |
| JSL Blackletter font | Ready — `JBLACK.TTF` |
| Bio | In progress — Sean + Richard |
| Band photo | TBD |
| EPK (PDF) | In progress |
| One Sheet (PDF) | In progress |

Items marked In Progress or TBD use placeholders at launch and drop in when ready.
