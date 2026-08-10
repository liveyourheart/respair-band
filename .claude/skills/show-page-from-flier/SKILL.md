---
name: show-page-from-flier
description: Use when adding a new show to the respair-band site from a flier image. Covers reading the flier, resolving uncertain band names, researching social links, copying the poster, creating the show page, and updating the shows list.
---

# Show Page from Flier

## Overview

End-to-end workflow for turning a show flier into a show page on respair.band. Covers every step from image read to commit.

## Steps

### 1. Read the flier

Use the Read tool on the image path the user provides. Extract:
- Date (month, day, year)
- Venue name and city
- Show time, age restriction, ticket price
- All band names
- Presenter (e.g. "Zero Tolerance Presents")

### 2. Confirm uncertain band names

Stylized blackletter and custom logotypes are hard to read. Before doing anything else, tell the user which names you're unsure about and your best guess. Get confirmation. Don't assume.

### 3. Research social links

Search for each non-Respair band in parallel. Priority order for links:
1. Bandcamp
2. Instagram
3. Facebook
4. Linktree / website

If a band can't be found (very local/underground), leave `links={[]}` — don't add placeholder text.

Note: the venue gets its own `BandBlock` with any relevant link if found.

### 4. Copy the flier as the poster

```bash
cp /path/to/flier.jpg /Users/mm/PRP/respair-band/public/shows/MMDDYYYY.jpg
```

The filename is the date in `MMDDYYYY` format with no separators, lowercase `.jpg`.

### 5. Create the show page

File: `src/pages/shows/MMDDYYYY.astro`

```astro
---
import ShowLayout from '../../layouts/ShowLayout.astro';
import BandBlock from '../../components/BandBlock.astro';
---

<ShowLayout
  title="MM-DD-YYYY"
  description="Respair with [bands] at [Venue], [City] — [Month Day, Year]."
  poster="/shows/MMDDYYYY.jpg"
  accentColor="#XXXXXX"
  bgColor="#XXXXXX"
  posterWidth="40%"
  posterWidthMobile="85%"
>
  <BandBlock name="HEADLINER" links={[...]} />
  <BandBlock name="BAND TWO" links={[...]} />
  <BandBlock name="RESPAIR" links={[
    { label: 'Website', url: '/' },
    { label: 'Instagram', url: 'https://www.instagram.com/a_moment_of_respair/' },
  ]} />
  <BandBlock name="Venue Name — City, ST" links={[...]} />
  <BandBlock name="Tickets" links={[
    { label: '$X // 21+ // X:00PM', url: '' },
  ]} />
</ShowLayout>
```

**Color picking:** Sample the flier's dominant background and accent colors. Use a dark near-black for `bgColor` and the flier's primary highlight color for `accentColor`. When in doubt, `#0a0a0a` / `#774F17` are safe fallbacks.

**Poster width:** Fliers are portrait — `40%` web, `85%` mobile works well. Adjust if the flier has a lot of horizontal content.

**Band order:** List headliner first, Respair in its actual billing position, venue and ticket info last.

### 6. Update the shows list in index.astro

Add an entry to the `shows` array in `src/pages/index.astro`. Insert in reverse-chronological order (newest upcoming at top):

```js
{ date: 'YYYY-MM-DD', slug: 'MMDDYYYY', display: 'MM/DD/YYYY — City, ST — Venue Name' },
```

Use the full city and state abbreviation in `display` when the show is out of OKC.

### 7. Check it in the browser

Run `npm run dev`, navigate to `/#shows` to confirm the entry appears under Upcoming, then click through to the show page to verify the poster loads and band blocks render correctly.

### 8. Commit and push

Stage: the new `.astro` file, the poster `.jpg`, and `index.astro`.

```
feat: add MM/DD/YYYY show at [Venue], [City]

[Headliner] / [Band] / Respair / [Band]
[Presenter] — $X, 21+, X:00PM
```

## Common Mistakes

- **Copying the flier to `dist/`** — `dist/` is build output, it gets wiped. Always copy to `public/shows/`.
- **Wrong filename format** — must be `MMDDYYYY.jpg` (no dashes, no underscores).
- **Guessing band name spellings** — always confirm with the user before building the page.
- **Leaving city as OKC for out-of-town shows** — update both the `display` string and the venue BandBlock name to include actual city and state.
