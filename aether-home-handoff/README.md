# Aether Home Page — Interaction Prototype / Handoff

> **Read this first:** This is a **behavior & interaction spec**, not production code.
> Engineers should read it to see how each section looks, animates, and responds — then
> lift the logic into the production stack. Design decisions are canonical in the Figma
> file (`Aether Tokens` collection); this prototype is the reference for behavior.

**Live preview:** *(Netlify URL — updates on every push once configured)*

## Structure

```
aether-home-handoff/
├── index.html               Shell: nav, iframe includes for each section, height sync
├── sections/
│   ├── 01-hero.html         Cinematic reframe — "One Blueprint. Every Factory."
│   ├── 02-pedigree.html     Preserved-elevated — "Where our team comes from"
│   ├── 03-shift.html        Editorial manifesto — "Aether runs on software"
│   ├── 04-blueprint.html    Brand-teal vision — exploded APG/FacOS/QMS stack
│   ├── 05-storymap.html     Signature moment — animated global → US-first arc
│   ├── 06-inproduction.html  Sector selector + part output — "not a concept"
│   ├── 07-quoteflow.html    Formlabs-style walkthrough — upload → understand → order
│   ├── 08-testimonials.html Brand-teal spotlight — engineer voices
│   └── 09-cta.html          Segmented CTA — three doors (parts / engineer / investor)
├── styles/
│   └── tokens.css           Canonical Aether design tokens (mirrors Figma variables)
└── assets/
    └── images/              Shared with Technology page — factory + part photos
```

## Section rhythm (locked in v0.3 flow prototype)

Nav · **Hero (ink)** · Pedigree (paper) · Shift (ink) · **Blueprint (brand-teal)**
· Story map (paper→ink split) · In production (ink) · Quote flow (paper)
· **Testimonials (brand-teal)** · CTA (ink-950) · Footer

The brand-teal sections at Blueprint (04) and Testimonials (08) bracket the product
journey between two "brand voice" moments — vision claim, then engineer proof.

## Running locally

Sections load via `<iframe src="sections/…">`, so serve over HTTP (opening `index.html`
from the filesystem may break the height sync):

```bash
npx serve .          # or: python -m http.server 8000
```

VS Code users: right-click `index.html` → **Open with Live Server**.

## For the engineering team

- The iframe-per-section architecture is a **prototyping convenience** — each section's
  CSS/JS is isolated during design iteration. In production, sections become components
  in your framework of choice; the per-section `<style>` and `<script>` blocks map 1:1
  to component styles and behavior.
- The height-reporting snippet (`postMessage`) at the end of each section can be dropped
  in production.
- Fonts: Montserrat via Google Fonts; mono stack defined in `--mono`.
- The "Get a quote" CTA points to `https://quote.aethermfg.com/login`.
- `@media (prefers-reduced-motion: reduce)` should be honored in the port.

## Related

- **Technology page**: `../aether-technology-handoff/` — the sister package.
- **Figma**: `Aether Quote System — Copy` file (`Jgf4b6AiQLZns49vfGv2B4`), `Aether Tokens`
  collection is canonical for colors, type, and spacing.
