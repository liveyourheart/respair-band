# Respair Site Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure `respair.band` from a single-scroll page into a four-tab layout (Band, Shows, Press, Contact) and add all missing identity content.

**Architecture:** `index.astro` is restructured to keep the logo and streaming links always visible, with a vanilla JS tab switcher below that reads/writes `window.location.hash`. All tab content lives in `index.astro`. CSS changes are contained to `respair.css`. Show pages get description props threaded through `ShowLayout` → `BaseLayout`.

**Tech Stack:** Astro 6, vanilla JS, CSS custom properties. No test framework — verification is `npm run build` (zero errors) plus visual check in `npm run dev`.

---

## Files

| Action | Path | Purpose |
|---|---|---|
| Create | `.gitignore` | Exclude build artifacts and tooling dirs |
| Create | `public/press/.gitkeep` | Reserve directory for future EPK/one-sheet PDFs |
| Modify | `src/styles/respair.css` | Add `@font-face` stub, tab nav styles, tab section visibility |
| Modify | `src/pages/index.astro` | Full restructure: tab nav + four tab sections + inline JS |
| Modify | `src/layouts/ShowLayout.astro` | Thread `description` prop through to `BaseLayout` |
| Modify | `src/pages/shows/08162025.astro` | Add meta description |
| Modify | `src/pages/shows/12072025.astro` | Add meta description |
| Modify | `src/pages/shows/01242026.astro` | Add meta description |
| Modify | `src/pages/shows/03292026.astro` | Add meta description |
| Modify | `src/pages/shows/04182026.astro` | Add meta description |
| Modify | `src/pages/shows/05162026.astro` | Add meta description |

---

## Task 1: Add .gitignore and press directory

**Files:**
- Create: `.gitignore`
- Create: `public/press/.gitkeep`

- [ ] **Step 1: Create .gitignore**

```
node_modules/
dist/
.astro/
.superpowers/
```

Save to `.gitignore` at the repo root.

- [ ] **Step 2: Create press directory placeholder**

Create an empty file at `public/press/.gitkeep`. This reserves the directory for EPK and one-sheet PDFs that will be added later.

- [ ] **Step 3: Verify build is clean**

```bash
npm run build
```

Expected: exits with no errors, `dist/` is produced.

- [ ] **Step 4: Commit**

```bash
git add .gitignore public/press/.gitkeep
git commit -m "chore: add .gitignore and press assets directory"
```

---

## Task 2: CSS — font face stub and tab nav styles

**Files:**
- Modify: `src/styles/respair.css`

The current `respair.css` uses `'Arial Black'` for all headings. This task adds the JSL Blackletter `@font-face` declaration (font file TBD — Arial Black remains the visible fallback until the file is dropped in) and adds all styles needed for the tab nav and tab sections.

- [ ] **Step 1: Add @font-face declaration and update heading font stacks**

Replace the existing `h1` and `h2` blocks and add the `@font-face` and tab styles. Full updated `src/styles/respair.css`:

```css
/* JSL Blackletter font — file dropped into public/fonts/ when available.
   Arial Black is the visible fallback until then. */
@font-face {
  font-family: 'JSL Blackletter';
  src: url('/fonts/JSLBlackletter.woff2') format('woff2'),
       url('/fonts/JSLBlackletter.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}

body {
    margin-bottom: 50px;
    background-color: #070707;
}

h1 {
    color: #774F17;
    font-family: 'JSL Blackletter', 'Arial Black', sans-serif;
    margin-bottom: 10px;
}

h2 {
    color: #774F17;
    font-family: 'JSL Blackletter', 'Arial Black', sans-serif;
    font-size: 0.8em;
    display: block;
    margin-left: auto;
    margin-right: auto;
}

.show-link {
    display: block;
}

.shows {
    font-size: 150%;
    text-align: center;
    margin-top: 30px;
}

a {
    color: #774F17;
}

.logo {
    display: block;
    margin-left: auto;
    margin-right: auto;
    width: 75%;
}

.link-container {
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    margin-top: 20px;
    margin-bottom: 20px;
    flex-wrap: wrap;
}

.link-img {
    padding: 10px;
    font-size: 30px;
    text-align: center;
}

#apple {
    margin-top: -10px;
}

#youtube {
    margin-top: 10px;
}

/* Tab navigation */
.tab-nav {
    display: flex;
    justify-content: center;
    gap: 30px;
    margin: 10px 0 30px;
    font-family: 'JSL Blackletter', 'Arial Black', sans-serif;
    font-size: 1.3em;
}

.tab-nav a {
    color: #444;
    text-decoration: none;
    cursor: pointer;
}

.tab-nav a.active {
    color: #774F17;
}

/* Tab sections — hidden by default; JS shows the active one */
.tab-section {
    display: none;
    text-align: center;
}

/* Band tab */
.band-bio {
    color: #aaa;
    font-size: 1em;
    line-height: 1.7;
    max-width: 500px;
    margin: 0 auto 20px;
    text-align: left;
}

.band-benefit {
    color: #888;
    font-size: 0.9em;
    margin-bottom: 20px;
}

/* Press tab */
.press-quote {
    font-style: italic;
    font-size: 1.05em;
    color: #ccc;
    max-width: 500px;
    margin: 0 auto 6px;
    text-align: left;
    line-height: 1.6;
}

.press-attribution {
    font-family: 'JSL Blackletter', 'Arial Black', sans-serif;
    font-size: 0.9em;
    color: #774F17;
    max-width: 500px;
    margin: 0 auto 24px;
    text-align: left;
}

.press-item {
    display: block;
    margin-bottom: 8px;
    font-size: 1em;
}

.press-pending {
    color: #444;
    font-style: italic;
    font-size: 0.9em;
    margin-bottom: 8px;
    display: block;
}

/* Contact tab */
.contact-email {
    margin-bottom: 20px;
    font-size: 1em;
}

.contact-form input,
.contact-form textarea {
    display: block;
    width: 50vw;
    background-color: #0e0e0e;
    border: 2px solid #774F17;
    border-radius: 4px;
    color: #ccc;
    font-size: 1em;
    padding: 8px 12px;
    margin: 10px auto;
    box-sizing: border-box;
}

.contact-form textarea {
    min-height: 120px;
    resize: vertical;
}

.contact-form button {
    display: block;
    margin: 10px auto;
    background-color: #774F17;
    color: #070707;
    border: none;
    padding: 10px 30px;
    font-family: 'JSL Blackletter', 'Arial Black', sans-serif;
    font-size: 1em;
    cursor: pointer;
}

#success-message {
    color: #774F17;
    font-family: 'JSL Blackletter', 'Arial Black', sans-serif;
    text-align: center;
    font-size: 1.2em;
    margin: 20px auto;
}

@media (max-width: 768px) {
    .contact-form input,
    .contact-form textarea {
        width: 85vw;
    }

    body {
        font-size: 100%;
        margin-bottom: 30px;
    }

    h1 {
        font-size: 1.5em;
    }

    h2 {
        font-size: 0.7em;
    }

    .shows {
        font-size: 100%;
    }

    .logo {
        width: 100%;
    }

    .tab-nav {
        font-size: 1.1em;
        gap: 20px;
    }

    .link-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
    }

    .link-img {
        width: 50%;
        margin-top: 15px;
        margin-bottom: 15px;
        display: block;
        margin-left: auto;
        margin-right: auto;
    }
}
```

- [ ] **Step 2: Verify build**

```bash
npm run build
```

Expected: exits with no errors.

- [ ] **Step 3: Commit**

```bash
git add src/styles/respair.css
git commit -m "style: add tab nav styles and JSL Blackletter font-face stub"
```

---

## Task 3: Restructure index.astro with four-tab layout

**Files:**
- Modify: `src/pages/index.astro`

This replaces the current single-scroll page with the tab structure. Logo and streaming links remain always-visible above the tabs. The vanilla JS reads `window.location.hash` on load and wires up click handlers and `popstate`.

**Before starting:** You need the Keep Oklahoma Heavy article URL to use as the `href` on the press quote attribution. If you don't have it, use `href="https://keepoklahomahevy.com"` as a placeholder and add a `<!-- TODO: replace with actual KOH article URL -->` comment.

- [ ] **Step 1: Replace index.astro**

Full replacement of `src/pages/index.astro`:

```astro
---
import BaseLayout from '../layouts/BaseLayout.astro';
import StreamingLinks from '../components/StreamingLinks.astro';
import ContactForm from '../components/ContactForm.astro';
import '../styles/respair.css';
---

<BaseLayout
  title="Respair"
  description="Respair is an atmospheric emo crust band from Oklahoma City."
  ogImage="https://respair.band/images/preview.png"
  ogUrl="https://respair.band/"
>
  <img class="logo" src="/images/website-logo.png" alt="Respair" />

  <StreamingLinks
    links={[
      { url: 'https://music.apple.com/us/album/demo-2025-ep/1858472615', icon: '/images/apple-logo.png', id: 'apple' },
      { url: 'https://amomentofrespair.bandcamp.com/album/demo-2025-2', icon: '/images/bandcamp.png' },
      { url: 'https://www.instagram.com/a_moment_of_respair', icon: '/images/instagram.png' },
      { url: 'https://open.spotify.com/album/02VUgTNphFEAQ3bsFKbXjH?si=o9cpXsKBQpGzDswLn6Rs0Q', icon: '/images/spotify.png' },
      { url: 'https://www.youtube.com/watch?v=Ki7e_ppGh44&list=OLAK5uy_kQSh49Otdft_VY4WZcnLhAJBl191pep9A', icon: '/images/youtube.png', id: 'youtube' },
    ]}
  />

  <nav class="tab-nav">
    <a href="#band">Band</a>
    <a href="#shows">Shows</a>
    <a href="#press">Press</a>
    <a href="#contact">Contact</a>
  </nav>

  <!-- BAND -->
  <div id="tab-band" class="tab-section">
    <p class="band-bio">
      <!-- Bio pending — Sean and Richard are working on it. Replace this comment with the bio paragraph when delivered. -->
    </p>
    <p class="band-benefit">Respair has performed benefit shows for The Homeless Alliance and Pivot OKC.</p>
    <!-- Band photo: add <img> here when available -->
  </div>

  <!-- SHOWS -->
  <div id="tab-shows" class="tab-section">
    <div class="shows">
      <h1>Shows</h1>
      <a class="show-link" href="/shows/04182026/">04/18/2026 - OKC - 89th Street</a><br />
      <a class="show-link" href="/shows/05162026/">05/16/2026 - OKC - Resonant Head</a><br />
      <h2>Past Shows</h2>
      <a class="show-link" href="/shows/03292026/">03/29/2026 - OKC - Resonant Head</a><br />
      <a class="show-link" href="/shows/01242026/">01/24/2026 - OKC - Resonant Head</a><br />
      <a class="show-link" href="/shows/12072025/">12/07/2025 - OKC - The Pet Shop</a><br />
      <a class="show-link" href="/shows/08162025/">08/16/2025 - OKC - Hooligan's</a><br />
    </div>
  </div>

  <!-- PRESS -->
  <div id="tab-press" class="tab-section">
    <p class="press-quote">"Everything you're missing in your collection, and your life… reminiscent of From Ashes Rise, Tragedy, Remains of the Day, and early Neurosis."</p>
    <p class="press-attribution">
      <!-- TODO: replace href with actual Keep Oklahoma Heavy article URL -->
      — <a href="https://keepoklahomahevy.com" target="_blank" rel="noopener">Keep Oklahoma Heavy</a>
    </p>
    <!-- Podcast mention: replace this comment with a link when the podcast is identified -->
    <span class="press-pending">Podcast mention coming soon</span>
    <!-- EPK: replace press-pending span with <a class="press-item" href="/press/epk.pdf">EPK</a> when file is ready -->
    <span class="press-pending">EPK coming soon</span>
    <!-- One sheet: replace press-pending span with <a class="press-item" href="/press/one-sheet.pdf">One Sheet</a> when file is ready -->
    <span class="press-pending">One Sheet coming soon</span>
    <p style="margin-top: 20px;">
      Press inquiries: <a href="mailto:amomentofrespair@gmail.com">amomentofrespair@gmail.com</a>
    </p>
  </div>

  <!-- CONTACT -->
  <div id="tab-contact" class="tab-section">
    <div class="contact-email">
      <a href="mailto:amomentofrespair@gmail.com">amomentofrespair@gmail.com</a>
    </div>
    <ContactForm
      accessKey="ea1405d3-0166-499a-9525-23b9e3b14187"
      subject="Respair Contact"
      redirectUrl="https://respair.band/?tab=contact&success=true"
    />
  </div>
</BaseLayout>

<script>
  const validTabs = ['band', 'shows', 'press', 'contact'];

  function activateTab(name: string) {
    document.querySelectorAll('.tab-section').forEach((s) => {
      (s as HTMLElement).style.display = 'none';
    });
    document.querySelectorAll('.tab-nav a').forEach((a) => a.classList.remove('active'));
    const section = document.getElementById('tab-' + name);
    const link = document.querySelector(`.tab-nav a[href="#${name}"]`);
    if (section) section.style.display = 'block';
    if (link) link.classList.add('active');
  }

  // Activate on load
  const hash = window.location.hash.slice(1);
  activateTab(validTabs.includes(hash) ? hash : 'band');

  // Tab clicks
  document.querySelectorAll('.tab-nav a').forEach((link) => {
    link.addEventListener('click', (e) => {
      e.preventDefault();
      const name = (link as HTMLAnchorElement).getAttribute('href')!.slice(1);
      history.pushState(null, '', '#' + name);
      activateTab(name);
    });
  });

  // Browser back/forward
  window.addEventListener('popstate', () => {
    const h = window.location.hash.slice(1);
    activateTab(validTabs.includes(h) ? h : 'band');
  });
</script>
```

**Note on ContactForm redirect URL:** The redirect is updated to `?tab=contact&success=true` so the success state returns to the Contact tab. The ContactForm success script in `ContactForm.astro` checks `window.location.search.includes('success=true')` — that still works. But the tab JS runs on load and will read the hash, not the query param, so the Contact tab won't auto-activate on success unless the URL also includes `#contact`. Update the redirectUrl to `https://respair.band/#contact?success=true` — but note query params after a hash aren't standard. Simplest fix: keep the redirect as `https://respair.band/?success=true` and update ContactForm.astro's success script to also activate the contact tab when it shows the success message. See Task 3, Step 2.

- [ ] **Step 2: Update ContactForm success script to activate contact tab**

In `src/components/ContactForm.astro`, replace the existing `<script>` block:

```astro
<script>
  if (window.location.search.includes('success=true')) {
    const form = document.querySelector('.contact-form') as HTMLElement | null;
    const msg = document.getElementById('success-message');
    if (form) form.style.display = 'none';
    if (msg) msg.style.display = 'block';

    // Activate the contact tab so the success message is visible
    const contactSection = document.getElementById('tab-contact');
    const contactLink = document.querySelector('.tab-nav a[href="#contact"]');
    document.querySelectorAll('.tab-section').forEach((s) => {
      (s as HTMLElement).style.display = 'none';
    });
    document.querySelectorAll('.tab-nav a').forEach((a) => a.classList.remove('active'));
    if (contactSection) contactSection.style.display = 'block';
    if (contactLink) contactLink.classList.add('active');
  }
</script>
```

Also keep the `redirectUrl` in `index.astro` as `https://respair.band/?success=true` (no change needed there).

- [ ] **Step 3: Verify in dev**

```bash
npm run dev
```

Open `http://localhost:4321`. Check:
- Logo and streaming links always visible
- Tabs switch on click
- URL hash updates on each click
- Loading `http://localhost:4321/#press` directly opens Press tab
- Loading `http://localhost:4321/#shows` directly opens Shows tab
- Band tab is the default when no hash is present
- Browser back/forward navigates between tabs

- [ ] **Step 4: Verify build**

```bash
npm run build
```

Expected: exits with no errors.

- [ ] **Step 5: Commit**

```bash
git add src/pages/index.astro src/components/ContactForm.astro
git commit -m "feat: restructure index with four-tab navigation (Band, Shows, Press, Contact)"
```

---

## Task 4: Thread description prop through ShowLayout

**Files:**
- Modify: `src/layouts/ShowLayout.astro`

`ShowLayout` currently passes only `title` to `BaseLayout`. `BaseLayout` already accepts an optional `description` prop — this task threads it through so show pages can set their own meta description.

- [ ] **Step 1: Update ShowLayout.astro**

Replace `src/layouts/ShowLayout.astro` with:

```astro
---
import BaseLayout from './BaseLayout.astro';
import '../styles/shows.css';

interface Props {
  title: string;
  description?: string;
  poster: string;
  accentColor?: string;
  bgColor?: string;
  posterWidth?: string;
  posterWidthMobile?: string;
}

const {
  title,
  description,
  poster,
  accentColor = '#907241',
  bgColor = '#070707',
  posterWidth = '66%',
  posterWidthMobile = '50%',
} = Astro.props;
---

<BaseLayout title={title} description={description}>
  <Fragment slot="head">
    <style set:html={`:root { --show-accent: ${accentColor}; --show-bg: ${bgColor}; --poster-width: ${posterWidth}; --poster-width-mobile: ${posterWidthMobile}; }`}></style>
  </Fragment>
  <img class="poster" src={poster} alt={title} />
  <div style="text-align: center;">
    <slot />
  </div>
</BaseLayout>
```

- [ ] **Step 2: Verify build**

```bash
npm run build
```

Expected: exits with no errors. No visible change yet — show pages haven't passed descriptions.

- [ ] **Step 3: Commit**

```bash
git add src/layouts/ShowLayout.astro
git commit -m "feat: thread description prop through ShowLayout to BaseLayout"
```

---

## Task 5: Add meta descriptions to all show pages

**Files:**
- Modify: `src/pages/shows/08162025.astro`
- Modify: `src/pages/shows/12072025.astro`
- Modify: `src/pages/shows/01242026.astro`
- Modify: `src/pages/shows/03292026.astro`
- Modify: `src/pages/shows/04182026.astro`
- Modify: `src/pages/shows/05162026.astro`

- [ ] **Step 1: Update 08162025.astro**

```astro
---
import ShowLayout from '../../layouts/ShowLayout.astro';
---

<ShowLayout
  title="08-16-2025"
  description="Respair live at Hooligan's, Oklahoma City — August 16, 2025."
  poster="/shows/08162025.jpg"
/>
```

- [ ] **Step 2: Update 12072025.astro**

```astro
---
import ShowLayout from '../../layouts/ShowLayout.astro';
import BandBlock from '../../components/BandBlock.astro';
---

<ShowLayout
  title="12-07-2025"
  description="Respair with Naturalist, Speak Memory, and Earthfucker at The Pet Shop, Oklahoma City — December 7, 2025."
  poster="/shows/12072025.png"
  posterWidth="33%"
  posterWidthMobile="50%"
>
  <BandBlock
    name="NATURALIST"
    links={[
      { label: 'Bandcamp', url: 'https://naturalistmusic.bandcamp.com/' },
      { label: 'Instagram', url: 'https://www.instagram.com/naturalistmusic/' },
    ]}
  />
  <BandBlock
    name="SPEAK, MEMORY"
    links={[
      { label: 'speakmemoryok.com', url: 'https://speakmemoryok.com/' },
      { label: 'Instagram', url: 'https://www.instagram.com/speakmemoryok/' },
    ]}
  />
  <BandBlock
    name="EARTHFUCKER"
    subtitle="Oklahoma Crust Revival"
    links={[
      { label: 'Bandcamp', url: 'https://earthfucker420.bandcamp.com/album/why-should-you-survive' },
    ]}
  />
  <BandBlock
    name="RESPAIR"
    links={[
      { label: 'Bandcamp', url: 'https://amomentofrespair.bandcamp.com/album/demo-2025-2' },
      { label: 'Instagram', url: 'https://www.instagram.com/a_moment_of_respair/' },
    ]}
  />
  <BandBlock
    name="The Pet Shop"
    links={[
      { label: 'thepetshopokc.com', url: 'https://www.thepetshopokc.com/' },
    ]}
  />
  <BandBlock
    name="Can't Make The Show?"
    links={[
      { label: 'Donate to the Oklahoma City Homeless Alliance directly', url: 'https://www.homelessalliance.org/donate' },
    ]}
  />
</ShowLayout>
```

- [ ] **Step 3: Update 01242026.astro**

```astro
---
import ShowLayout from '../../layouts/ShowLayout.astro';
import BandBlock from '../../components/BandBlock.astro';
---

<ShowLayout
  title="01-24-2026"
  description="Respair with Bugnog, Ectospire, and My Witch My Blood at Resonant Head, Oklahoma City — January 24, 2026."
  poster="/shows/01242026.jpg"
  accentColor="#FFFEFF"
  bgColor="#010101"
  posterWidth="33%"
  posterWidthMobile="50%"
>
  <BandBlock
    name="BUGNOG"
    links={[
      { label: 'Bandcamp', url: 'https://bugnog.bandcamp.com/album/american-dreamed' },
      { label: 'Instagram', url: 'https://www.instagram.com/bugnogband/?hl=en' },
    ]}
  />
  <BandBlock
    name="ECTOSPIRE"
    links={[
      { label: 'Linktree', url: 'https://linktr.ee/ectospiredeath' },
      { label: 'Instagram', url: 'https://www.instagram.com/ectospire' },
    ]}
  />
  <BandBlock
    name="MY WITCH MY BLOOD"
    links={[
      { label: 'Bandcamp', url: 'https://mwmbdoom.bandcamp.com/' },
      { label: 'Instagram', url: 'https://www.instagram.com/mwmbdoom/' },
    ]}
  />
  <BandBlock
    name="RESPAIR"
    links={[
      { label: 'Website', url: '/' },
      { label: 'Instagram', url: 'https://www.instagram.com/a_moment_of_respair/' },
    ]}
  />
  <BandBlock
    name="Resonant Head"
    links={[
      { label: 'resonanthead.com', url: 'https://www.resonanthead.com/' },
    ]}
  />
  <BandBlock
    name="Tickets"
    links={[
      { label: 'Prekindle', url: 'https://www.prekindle.com/event/63983-bugnog-ectospire-mywitchmyblood-respair-oklahoma-city' },
    ]}
  />
</ShowLayout>
```

- [ ] **Step 4: Update 03292026.astro**

```astro
---
import ShowLayout from '../../layouts/ShowLayout.astro';
import BandBlock from '../../components/BandBlock.astro';
---

<ShowLayout
  title="03-29-2026"
  description="Respair with Bugnog, Ectospire, and My Witch My Blood at Resonant Head, Oklahoma City — March 29, 2026."
  poster="/shows/03292026.png"
  accentColor="#FFFEFF"
  bgColor="#010101"
  posterWidth="33%"
  posterWidthMobile="50%"
>
  <BandBlock
    name="BUGNOG"
    links={[
      { label: 'Bandcamp', url: 'https://bugnog.bandcamp.com/album/american-dreamed' },
      { label: 'Instagram', url: 'https://www.instagram.com/bugnogband/?hl=en' },
    ]}
  />
  <BandBlock
    name="ECTOSPIRE"
    links={[
      { label: 'Linktree', url: 'https://linktr.ee/ectospiredeath' },
      { label: 'Instagram', url: 'https://www.instagram.com/ectospire' },
    ]}
  />
  <BandBlock
    name="RESPAIR"
    links={[
      { label: 'Website', url: '/' },
      { label: 'Instagram', url: 'https://www.instagram.com/a_moment_of_respair/' },
    ]}
  />
  <BandBlock
    name="MY WITCH MY BLOOD"
    links={[
      { label: 'Bandcamp', url: 'https://mwmbdoom.bandcamp.com/' },
      { label: 'Instagram', url: 'https://www.instagram.com/mwmbdoom/' },
    ]}
  />
  <BandBlock
    name="Resonant Head"
    links={[
      { label: 'resonanthead.com', url: 'https://www.resonanthead.com/' },
    ]}
  />
  <BandBlock
    name="Tickets"
    links={[
      { label: 'Prekindle', url: 'https://www.prekindle.com/event/63983-bugnog-ectospire-mywitchmyblood-respair-oklahoma-city' },
    ]}
  />
</ShowLayout>
```

- [ ] **Step 5: Update 04182026.astro**

```astro
---
import ShowLayout from '../../layouts/ShowLayout.astro';
import BandBlock from '../../components/BandBlock.astro';
---

<ShowLayout
  title="04-18-2026"
  description="Respair with lowheaven, Drosera, and itallhappenedinjuly at 89th Street, Oklahoma City — April 18, 2026."
  poster="/shows/04182026.jpg"
  accentColor="#7FA99A"
  bgColor="#131A11"
  posterWidth="33%"
  posterWidthMobile="50%"
>
  <BandBlock
    name="lowheaven"
    links={[
      { label: 'Bandcamp', url: 'https://lowheavenband.bandcamp.com/album/ritual-decay' },
      { label: 'Instagram', url: 'https://www.instagram.com/lowheavenband/' },
    ]}
  />
  <BandBlock
    name="DROSERA"
    links={[
      { label: 'Bandcamp', url: 'https://drosera.bandcamp.com/' },
      { label: 'Instagram', url: 'https://www.instagram.com/droserafl/' },
    ]}
  />
  <BandBlock
    name="RESPAIR"
    links={[
      { label: 'Website', url: '/' },
      { label: 'Instagram', url: 'https://www.instagram.com/a_moment_of_respair/' },
    ]}
  />
  <BandBlock
    name="itallhappenedinjuly"
    links={[
      { label: 'Bandcamp', url: 'https://itallhappenedinjuly.bandcamp.com/' },
      { label: 'Instagram', url: 'https://www.instagram.com/itallhappenedinjuly/' },
    ]}
  />
  <BandBlock
    name="Venue"
    links={[
      { label: '89th Street OKC', url: 'https://www.89thstreetokc.com' },
    ]}
  />
</ShowLayout>
```

- [ ] **Step 6: Update 05162026.astro**

```astro
---
import ShowLayout from '../../layouts/ShowLayout.astro';
import BandBlock from '../../components/BandBlock.astro';
---

<ShowLayout
  title="05-16-2026"
  description="Respair with My Sweetest Kill and Commander Pilot at Resonant Head, Oklahoma City — May 16, 2026."
  poster="/shows/05162026.jpg"
  accentColor="#FFFEFF"
  bgColor="#010101"
  posterWidth="33%"
  posterWidthMobile="50%"
>
  <BandBlock
    name="MY SWEETEST KILL"
    subtitle="Cheap Science Fiction Album Release"
    links={[
      { label: 'mysweetestkill.com', url: 'https://mysweetestkill.com/home' },
    ]}
  />
  <BandBlock
    name="RESPAIR"
    links={[
      { label: 'Website', url: '/' },
      { label: 'Instagram', url: 'https://www.instagram.com/a_moment_of_respair/' },
    ]}
  />
  <BandBlock
    name="COMMANDER PILOT"
    links={[
      { label: 'Linktree', url: 'https://linktr.ee/commanderpilot' },
    ]}
  />
  <BandBlock
    name="Resonant Head"
    links={[
      { label: 'resonanthead.com', url: 'https://www.resonanthead.com/' },
    ]}
  />
  <BandBlock
    name="Tickets"
    links={[
      { label: 'Prekindle', url: 'https://www.prekindle.com/promo/id/-2852852648496939853' },
    ]}
  />
</ShowLayout>
```

- [ ] **Step 7: Verify build**

```bash
npm run build
```

Expected: exits with no errors.

- [ ] **Step 8: Spot-check meta tags**

```bash
grep -r "og:description" dist/shows/
```

Expected: six files each containing a `<meta property="og:description"` tag with show-specific text.

- [ ] **Step 9: Commit**

```bash
git add src/pages/shows/
git commit -m "feat: add meta descriptions to all show pages"
```

---

## Post-Launch Checklist (not in this repo)

- [ ] Generate a new Web3Forms access key for respair.band and update `accessKey` in `src/pages/index.astro` (currently using the key from the Destin for Dirt site)

These items are out of scope for this implementation but must happen around launch:

- [ ] Set up 301 redirect: `destinedfordirt.com/respair/` → `respair.band`
- [ ] Update `destinedfordirt.com` hub page link to point to `respair.band`
- [ ] Drop in JSL Blackletter font files (`public/fonts/JSLBlackletter.woff2` and `.ttf`) — no code change needed, `@font-face` is already declared
- [ ] Replace bio placeholder comment in `index.astro` Band tab with actual copy from Sean and Richard
- [ ] Add band photo `<img>` in Band tab when available
- [x] Podcast link — Killing Time Podcast by Kill Murray, linked at `t=1712s`
- [ ] Replace EPK `press-pending` span with `<a class="press-item" href="/press/epk.pdf">EPK</a>` and drop PDF into `public/press/`
- [ ] Replace One Sheet `press-pending` span with `<a class="press-item" href="/press/one-sheet.pdf">One Sheet</a>` and drop PDF into `public/press/`
- [x] KOH article URL — https://keepoklahomaheavy.substack.com/p/respair-the-return-of-hope
- [ ] Review Bandcamp tags (outside this repo)
