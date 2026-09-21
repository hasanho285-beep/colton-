# CLAUDE.md

Guidance for Claude (or any future contributor) working on this project.

## Project

A single-file, self-contained website for **M. Colton Comeaux** — a cinematic
"digital art book" portfolio for a creative director / photographer, built as
one static HTML file with vanilla CSS and JS. No build step, no dependencies,
no backend.

- **Source file:** `colton-comeaux.html`
- **Live artifact:** https://claude.ai/artifact/CeUkoZ2tfEPUiBtjPTL6bX
- **Client's existing site (reference only, do not scrape):** https://www.mcoltoncomeaux.com/ (robots-disallowed — cannot be fetched automatically)

## Creative direction (do not drift from this)

- Cinematic digital art book / visual archive, not a conventional portfolio.
- No card grids, no rounded corners, no gradients, no SaaS aesthetic.
- Identity phrase **"CALL ME COLTON."** recurs across the site (hero, vertical
  sticky mark, About headline, type-break interludes between projects) — it's
  a running visual motif, not just a homepage title.
- Palette: near-black ink (`#121110`), off-white paper (`#f2ede3`), muted
  stone for metadata. Color is meant to come from photography, not UI chrome.
- Type: **Fraunces** (italic serif) for all display/headlines, **Archivo**
  (grotesk) for body copy and tiny tracked-uppercase metadata
  (`PROJECT / CLIENT / YEAR / ROLE`).
- Motion should read as cinematic, not "tech demo": one orchestrated intro
  reveal per session, scroll-reveals that fire once, hover drift on images, a
  custom desktop cursor that swaps to VIEW / OPEN / PLAY. Everything respects
  `prefers-reduced-motion`.

## How the file is organized

Everything lives in `colton-comeaux.html`:

1. `<style>` — design tokens as CSS variables at the top (`--ink`, `--paper`,
   `--serif`, `--sans`, type scale via `clamp()`), then component styles.
2. Static shell in `<body>` — nav, mobile menu, grain overlay, intro veil,
   custom cursor, the recurring vertical "CALL ME COLTON." mark, `#app`
   (router mount point), footer.
3. `<script>` — a small hash-based router (`#/`, `#/work`, `#/work/:slug`,
   `#/about`, `#/archive`, `#/contact`) that renders template-literal HTML
   into `#app`. No frameworks.

### Content data (edit here, not in the templates)

Near the top of the script:

- `PROJECTS` — array of the 7 sample projects (`slug, title, client, year,
  role, tone, desc, images[]`). **These are placeholder names and copy**
  (Nocturne, Fieldwork, Glasswork, Periphery, Afterglow, Marrow, Low Tide) —
  swap in real project data here. `tone` picks a duotone gradient from the
  `TONES` map; `images[].type` controls the layout slot in the project-page
  sequence (`seq-full`, `seq-left`, `seq-right`, `seq-offset`).
- `CAPABILITIES` — About page skill list.
- `ARCHIVE_ITEMS` — Archive/notebook grid entries.
- `ABOUT_PORTRAIT` — a base64 data-URI JPEG (Colton's real photo, supplied by
  the client, cropped from an Instagram screenshot) used in the About page.
- The nav avatar (small circular photo next to "COLTON" in the header) is a
  separate inline base64 `<img>` in the HTML markup, not in the JS data.

### Real content already in place

- Real email: `mcoltoncomeaux@gmail.com`
- Real Instagram: https://www.instagram.com/mcoltoncomeaux/
- Real LinkedIn: https://www.linkedin.com/in/mcoltoncomeaux (title only —
  LinkedIn blocks automated fetches, so profile body copy was never pulled)
- Real emphasis line: "New York City — Working Worldwide" (About page + Contact)
- Two real photos of Colton (nav avatar, About portrait) — both supplied by
  the client as uploads and cropped/embedded as base64 JPEGs.

### Still placeholder — needs real client input

- All 7 project names/clients/years/roles/descriptions.
- All project and archive imagery — currently CSS/SVG duotone gradient
  panels standing in for photography (`.media` blocks), not real photos.

## Environment constraints that shaped this build

- Published artifacts can only load external resources from a fixed allow-list
  (fonts.googleapis.com / fonts.gstatic.com for fonts; a short list of script
  CDNs). **Arbitrary external images cannot be loaded at runtime** — this is
  why all photography is either an embedded base64 data URI or a generated
  placeholder, never a remote `<img src="https://...">`.
- The build sandbox itself has no outbound network access, so images can only
  get into the site by the user uploading the file directly in chat — URLs
  (e.g. a Squarespace CDN link) can't be downloaded automatically.
- Keep the file self-contained and under the artifact's 16 MB limit — each
  embedded photo should be compressed/resized (JPEG, ~1000px max edge) before
  base64-encoding, not dropped in at full camera resolution.

## Making changes

- Edit content in the data arrays/constants first; the templates read from
  them, so most copy/project changes don't touch layout code.
- Preserve the "/" separator for metadata and the uppercase tracked style —
  it's an explicit part of the brief.
- Don't reintroduce rounded corners, drop shadows as decoration, or card-grid
  layouts for `SELECTED WORK` — the asymmetric 12-column grid with varied
  spans and the type-break interludes are the point.
- After editing, sanity-check the inline script with `node --check` on the
  extracted `<script>` contents before republishing — the whole file has no
  build step to catch syntax errors otherwise.
- Republish via the Artifact tool using the existing artifact `url` so it
  updates in place rather than creating a new artifact.
