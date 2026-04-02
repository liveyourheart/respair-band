# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev      # local dev server (http://localhost:4321)
npm run build    # production build → dist/
npm run preview  # preview the dist/ build locally
```

No test suite. No linter configured.

## Architecture

Static Astro 6 site deployed to GitHub Pages via GitHub Actions (`.github/workflows/deploy.yml`). Builds to `dist/`, which is uploaded as the Pages artifact. `public/CNAME` contains `respair.band` so GitHub Pages serves the custom domain.

**Two page types:**

- `src/pages/index.astro` — Single-page app with four tabs (Band, Shows, Press, Contact). Tab switching is vanilla JS (~30 lines, inline at bottom of the file) using `window.location.hash` and `history.pushState`. All tab state lives in the URL hash. Default tab is Band.

- `src/pages/shows/*.astro` — One file per show, named by date (e.g. `04182026.astro`). Each uses `ShowLayout` with per-show `accentColor`, `bgColor`, `posterWidth`, and poster image path. Accent color and bg are injected as CSS custom properties (`--show-accent`, `--show-bg`) via an inline `<style>` tag in `ShowLayout`.

**Layouts:**
- `BaseLayout.astro` — HTML shell, meta/OG tags, favicon links
- `ShowLayout.astro` — Wraps BaseLayout; sets CSS custom properties from props, renders poster image, slots band blocks

**Components:**
- `BandBlock.astro` — Band name + list of links, used on show pages
- `StreamingLinks.astro` — Row of icon links (Apple Music, Bandcamp, Instagram, Spotify, YouTube)
- `ContactForm.astro` — Web3Forms contact form. Uses a test key in `DEV` mode; production key is passed as the `accessKey` prop in `index.astro`. On `?success=true` redirect, hides the form, shows a success message, and manually activates the Contact tab.

**Styles:**
- `src/styles/respair.css` — Main page styles. Brand color: `#774F17`. Font: JSL Blackletter (`public/fonts/JBLACK.TTF`) with `'Arial Black', sans-serif` fallback.
- `src/styles/shows.css` — Show page styles, uses `--show-accent` and `--show-bg` CSS custom properties.

## Pre-launch checklist

- [ ] Replace `accessKey` in `index.astro` ContactForm with a new Web3Forms key for respair.band
- [ ] Add bio copy (Band tab placeholder: `[ bio ]`)
- [ ] Add band photo (Band tab placeholder: `[ band photo ]`)
- [ ] Drop EPK PDF into `public/press/epk.pdf` and replace `press-pending` span in Press tab
- [ ] Drop one sheet PDF into `public/press/one-sheet.pdf` and replace `press-pending` span
- [ ] 301 redirect: `destinedfordirt.com/respair/` → `respair.band`
- [ ] Update Destin for Dirt hub page link to `respair.band`
