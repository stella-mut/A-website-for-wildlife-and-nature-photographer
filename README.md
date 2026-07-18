# Hamisi Studio

A modern, responsive one-page portfolio website for wildlife and nature photographer **Hamisi Sarah**, built around a custom, lightweight image gallery with a fullscreen lightbox — no frameworks, no build tools, no dependencies.

---

## Project Description

Hamisi Studio is a single self-contained `index.html` file (HTML, CSS, and vanilla JavaScript) that presents a photographer's portfolio in an editorial, field-notebook style. It's organized into ten sections:

1. **Hero** — full-bleed image, name, tagline, and a CTA into the portfolio
2. **Portfolio / Gallery** — filterable, masonry-style gallery across 8 collections (Wildlife, Birds, Mammals, Marine Life, Landscapes & Habitats, Macro & Small Wonders, Conservation Stories, Featured Expeditions)
3. **Featured Stories** — long-form photo essays, each opening as a curated image sequence
4. **About** — biography, philosophy, and specialties
5. **Conservation & Ethics** — field principles and supported organizations
6. **Prints & Licensing** — fine-art prints, licensing, editorial/commercial, publishing
7. **Publications / Exhibitions / Clients** — a selected-works table
8. **Journal** — field notes / blog-style cards
9. **Contact** — enquiry form and direct contact details
10. **Footer** — links, social, newsletter signup

The gallery itself is the core engineering piece: a custom-built (not plugin-based) responsive masonry grid with lazy-loaded images, a keyboard- and swipe-navigable fullscreen lightbox, scroll-triggered reveal animations, and per-image captions with location, species, and EXIF-style metadata.

**Color theme:** White (`#FFFFFF`) and Olive Green (`#556B2F`), with tonal tints/shades of each.
**Typeface:** Verdana, throughout, for both headings and body copy.

---

## Diagram / Preview

```
┌─────────────────────────────────────────┐
│  HEADER (sticky nav, transparent→solid)  │
├─────────────────────────────────────────┤
│  HERO — image, name, tagline, CTA        │
├─────────────────────────────────────────┤
│  PORTFOLIO — filter bar + masonry grid   │
│      └── click → LIGHTBOX (modal)        │
├─────────────────────────────────────────┤
│  FEATURED STORIES — essay cards          │
│      └── click → LIGHTBOX (story mode)   │
├─────────────────────────────────────────┤
│  ABOUT  │  CONSERVATION & ETHICS         │
├─────────────────────────────────────────┤
│  PRINTS & LICENSING │ PUBLICATIONS       │
├─────────────────────────────────────────┤
│  JOURNAL — field note cards              │
├─────────────────────────────────────────┤
│  CONTACT — details + form                │
├─────────────────────────────────────────┤
│  FOOTER — links, social, newsletter      │
└─────────────────────────────────────────┘
```

> The live page is a single scrolling document; the diagram above shows section order, not separate pages. Open `index.html` in a browser to see the real layout, colors, and gallery in action.

---

## Installation Instructions (Users)

No build step, package manager, or server is required.

1. Download `index.html`.
2. Double-click it, or drag it into a browser window — it opens locally as a static file (`file:///...`).
3. That's it. All styling and behavior is inline in the one file.

**To publish it online**, use any static host:

- **GitHub Pages** — push `index.html` to a repo, enable Pages in repo settings, done.
- **Netlify / Vercel** — drag-and-drop the file (or folder) onto their web dashboard.
- **Any web server** — upload `index.html` via FTP/SFTP to your hosting provider's public folder (e.g. `public_html/`).

No environment variables, API keys, or backend services are needed for the site to function.

---

## Instructions for Contributors (Developers)

### Project structure

```
index.html    ← everything: markup, <style>, and <script> in one file
README.md     ← this file
```

### Local development

1. Clone or download the project folder.
2. Open `index.html` directly in a browser — or better, serve it locally to avoid any `file://` quirks:
   ```bash
   # Python
   python3 -m http.server 8080
   # then visit http://localhost:8080
   ```
3. Edit `index.html` and refresh the browser. There is no build/compile step.

### Code layout inside `index.html`

- `<style>` block: CSS custom properties (`:root`) define the design tokens (colors, font, spacing) — change values here to restyle the whole site.
- `<body>`: sections are marked with `id`s matching the nav links (`#portfolio`, `#stories`, `#about`, etc.).
- `<script>`: a single IIFE containing:
  - `gallery` — array of portfolio image objects (category, title, location, species, EXIF)
  - `stories` — array of photo-essay objects, each with its own `images` array
  - `journal` — array of field-note cards
  - Rendering functions (`renderGallery`, story/journal card builders)
  - Lightbox logic (`openLightbox`, `lbStep`, keyboard/swipe handlers)
  - Header scroll state, mobile nav toggle, and form handlers

### Conventions

- Vanilla JS only — no framework, no build tooling. Keep new code dependency-free to preserve the "open the file and it works" simplicity.
- All data (images, captions, story text) lives in plain JS arrays near the top of `<script>` — treat this as the site's content layer.
- CSS uses BEM-ish, purpose-named classes (`.g-item`, `.g-cap`, `.lb-stage`) rather than utility classes; follow the existing naming style for new components.
- Respect `prefers-reduced-motion` — new animations should be covered by the existing media query at the bottom of the `<style>` block.

### Submitting changes

1. Fork the repository.
2. Create a feature branch: `git checkout -b feature/your-change`.
3. Make your changes directly in `index.html`.
4. Test in a real browser at both mobile (~375px) and desktop widths, and test the lightbox with keyboard (arrows/Esc) and touch (swipe).
5. Open a pull request describing what changed and why.

---

## How to Tweak This Project for Your Own Use

This template is easy to re-skin for a different photographer, artist, or portfolio niche.

**1. Swap the content**
- Replace the `gallery`, `stories`, and `journal` arrays in the `<script>` with your own images and text.
- Each gallery item needs: `cat` (category key), `seed` (unique image identifier), `h` (height, controls masonry rhythm), `title`, `location`, `species`, `exif`.
- Replace all `https://picsum.photos/seed/...` URLs with real image URLs (your own hosted photos, an S3 bucket, Cloudinary, etc.) once you have final assets — the seeded Picsum images are placeholders only.

**2. Change the categories**
- Edit the `CATS` object and the buttons inside `#filterBar` to match your own collections. Keep the `data-filter` attribute values in sync with the `cat` values used in the `gallery` array.

**3. Restyle it**
- Every color is a CSS variable in `:root` (`--olive`, `--cream`, `--ink`, etc.) — change these to re-theme the entire site in one place.
- Change `--font` to swap the typeface site-wide.

**4. Reorder or remove sections**
- Sections are independent `<section>` blocks with their own `id`. Delete a section and its matching nav link (`<a href="#id">`) to remove it; reorder by moving the whole `<section>...</section>` block.

**5. Connect the forms**
- The contact and newsletter forms currently just show a confirmation message (no backend). To make them functional, point the `<form>` at a service like Formspree, Netlify Forms, or your own endpoint, and replace the `preventDefault` handlers in the script accordingly.

**6. Add real EXIF/species data**
- If you have actual camera EXIF data, populate the `exif` field per image (e.g. `"600mm · f/5.6 · 1/1000s"`) to keep the field-label aesthetic accurate rather than illustrative.

---

## Expectations from Contributors

- **Keep it dependency-free.** No frameworks, no npm packages, no build pipeline — the project's value is that anyone can open the raw file and it works.
- **Keep it a single file (or a clearly justified split).** If a change genuinely requires splitting CSS/JS out, discuss it in the PR first — this is a deliberate design constraint, not an oversight.
- **Preserve accessibility.** Keep visible keyboard focus states, `aria-*` attributes on interactive elements (nav toggle, lightbox, close/prev/next buttons), and the `prefers-reduced-motion` fallback.
- **Test responsively.** Verify changes at mobile (~375px), tablet (~768px), and desktop (~1280px+) widths, including the mobile nav and the gallery's column count changes.
- **Test the gallery interactions directly**, not just visually: keyboard arrows and Esc in the lightbox, touch swipe on a real or emulated mobile device, and the category filter buttons.
- **Match the existing tone and design language.** Copy should stay editorial and field-note in voice; new UI should reuse the existing "field tag" / eyebrow pattern rather than introducing a new visual system.
- **No tracking or ad scripts.** This is a portfolio site for an individual artist — keep third-party scripts to genuinely necessary services (e.g. a form backend) and disclose them in the PR.

---

## Known Issues

- **Placeholder imagery.** All images currently load from `picsum.photos` and are unrelated to the actual subject matter (they are not real wildlife photos). These must be replaced with Hamisi's real photography before the site is used in production.
- **Forms have no backend.** The contact form and newsletter signup only display a static confirmation message on submit — no email is actually sent and no data is stored anywhere. This needs to be wired to a real form service or backend before launch.
- **No pagination / "load more" on the gallery.** All portfolio images render at once (filtered by category via `display`/DOM removal); a very large collection (100+ images) may benefit from adding pagination or infinite scroll, which is not currently implemented.
- **Story viewer reuses the main lightbox.** Photo essays open in the same lightbox component as the portfolio gallery; there's no dedicated "story mode" chrome (e.g. essay narrative text isn't shown alongside the images in the lightbox itself, only the image captions are).
- **No CMS or admin interface.** All content (images, text, captions) is hardcoded in JavaScript arrays inside `index.html`. Any content update currently requires editing the file directly — there is no visual editor.
- **Verdana-only typography.** Because Verdana has a limited weight range (no true light/medium weights), all hierarchy is created through size, spacing, and case rather than weight variation. This is a deliberate brief constraint, not a bug, but it's worth knowing if you plan to extend the type system.
- **Picsum image consistency.** Seeded Picsum images are generally stable but are a third-party service outside this project's control; if `picsum.photos` changes or becomes unavailable, all placeholder images will break until real images are substituted.
