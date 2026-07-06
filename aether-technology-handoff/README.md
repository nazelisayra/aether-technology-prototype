# Aether — Technology Page (Interaction Prototype / Handoff)

> **Read this first:** This is a **behavior & interaction spec**, not production code.
> It exists so the engineering team can see exactly how each section should look,
> animate, and respond — and lift the logic into the production stack.
> Design decisions will be canonical in Figma once that file is shared; until then,
> this prototype is the reference.

**Live preview:** https://resplendent-taffy-05828e.netlify.app/

## Structure

```
aether-technology-handoff/
├── index.html              Shell: fixed nav, theme switcher, iframe height sync (~90 lines)
├── sections/
│   ├── 01-why.html         Hero + "why" section
│   ├── 02-system.html      System overview
│   ├── 03-floor.html       Factory floor
│   ├── 04-experience.html  Experience (light theme region)
│   ├── 05-production.html  In production / portfolio
│   ├── 06-blueprint.html   Blueprint + side rail navigation
│   └── 07-testimonials.html  Quotes + CTA + outro
├── styles/
│   └── tokens.css          Design tokens (reference extract — see note below)
└── assets/
    └── images/             27 images extracted from the original base64 blobs
```

### How it fits together

- `index.html` renders each section in an `<iframe src="sections/…">`. Sections are
  **fully standalone HTML documents** — open any one directly to work on it in isolation.
- Each section posts its rendered height to the parent via `postMessage`
  (`{aetherH, aetherId}`); the shell listens and resizes the iframes so the page
  scrolls as one continuous document.
- The fixed nav switches between dark/light themes based on which section is under it
  (`data-reg="dark|light"` on each `<section>` in `index.html`).
- Image paths inside sections are relative (`../assets/images/…`), so sections render
  correctly both standalone and inside the shell.

### `styles/tokens.css`

Auto-collected union of every `:root` custom property used across the shell and all
sections (colors, typography, spacing). **It is not linked by any page** — each section
keeps its own inline copy to stay standalone. Treat it as the canonical token list when
porting to the production design system.

## Running locally

Because sections load via `iframe src`, serve over HTTP (opening `index.html` from the
filesystem may break the height sync in some browsers):

```bash
npx serve .          # or: python -m http.server 8000
```

## Deploying

Drag the **whole folder** onto https://app.netlify.com/drop — it deploys as-is.
Or push to a GitHub repo and connect it to Netlify ("Import from GitHub") for
auto-deploys on every push.

## Notes for the engineering team

- The iframe-per-section architecture is a **prototyping convenience** (it kept each
  section's CSS/JS isolated during design iteration). In production, sections should
  become components in your framework of choice; the per-section `<style>` and
  `<script>` blocks map 1:1 to component styles and behavior.
- Each section's inline `<script>` contains its interaction logic (scroll/hover
  animations, the blueprint rail, etc.) plus the small height-reporting snippet at the
  end — the latter can be dropped entirely in production.
- Fonts: Montserrat via Google Fonts; monospace stack defined in `--mono`.
- The "Get a quote" CTA points to `https://quote.aethermfg.com/login`.
- `@media (prefers-reduced-motion: reduce)` is handled in every section — please keep
  that behavior in the port.
