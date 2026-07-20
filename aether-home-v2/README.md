# Aether Home v2

The Aether Manufacturing home page, v2 — an editable, framework‑free build.

`index.html` is a **single, plain HTML + CSS + vanilla‑JS file**. There is no build
step, no bundler, and no runtime dependency. Open it, edit it, ship it.

---

## Why this exists

The original design was exported as `Aether Home v2 Light.dc.html` — a **Claude Design**
container (`<x-dc>`, `{{template vars}}`, `sc-for`/`sc-if` loops, `style-hover`, and a
`support.js` React runtime). That file **only renders inside claude.ai/design** and is not
practical to hand‑edit.

`index.html` is a faithful 1:1 conversion of that design to standard web code so that
frontend developers (and non‑developers) can read and change it directly. The look and
every interaction are identical.

> `Aether Home v2 Light.dc.html` is kept in this folder **as a backup only**. Edit
> `index.html` — not the `.dc.html`.

---

## Files

```
aether-home-v2/
├─ index.html                  ← EDIT THIS. The whole page, one file.
├─ storymap.html               ← §08 “Story Map”, embedded via <iframe> (self‑contained)
├─ assets/                     ← images, logos, layer renders, and tokens.css
│  └─ tokens.css               ← the canonical colour palette (see “Palette” below)
├─ uploads/                    ← photography + logos referenced by the page
├─ README.md                   ← this file
└─ Aether Home v2 Light.dc.html  ← original Claude Design export (backup, do not edit)
```

## Run it locally

Any static file server works (needed so the `storymap.html` iframe and images load cleanly):

```bash
cd aether-home-v2
npx serve .          # then open the printed http://localhost:3000
# or:  python -m http.server 8000
```

Opening `index.html` directly (double‑click) mostly works too, but a server is
recommended because of the embedded iframe.

---

## How the file is organised

`index.html` reads top‑to‑bottom in three parts:

1. **`<head><style>`** — base styles, `@keyframes`, the responsive breakpoints, and the
   few interactive helper classes (`.os-item`, `.sec-pill`).
2. **`<body>`** — the page. Each section is wrapped in a big banner comment so you can
   jump to it with a search:

   | Search for        | Section                                   |
   |-------------------|-------------------------------------------|
   | `NAV (`           | Top navigation (dark ↔ light on scroll)   |
   | `01 · HERO`       | Hero                                      |
   | `02 · PEDIGREE`   | “Where our team comes from” logo band     |
   | `03 · SCALE`      | Scale & network                           |
   | `04 · THE SHIFT`  | Traditional vs Aether comparison          |
   | `05 · THE OPERATING SYSTEM` | The 5‑layer stack (interactive) |
   | `06 · IN PRODUCTION` | Sector switcher (interactive)          |
   | `07 · QUOTE FLOW` | Steps + web/mobile carousel (interactive) |
   | `08 · STORY MAP`  | Embedded prototype (iframe)               |
   | `09 · TESTIMONIALS` | Auto‑scrolling quote marquee            |
   | `09b · VIDEO BAND`| Full‑bleed “Modular Factories” banner     |
   | `10 · SEGMENTED CTA` | Three‑door CTA                         |
   | `11 · FOOTER`     | Footer                                    |

3. **`<script>`** — all interactivity, one small vanilla module, commented per section.

### Styling conventions

- **Inline styles, literal hex.** Styles live on each element (e.g. `color:#135B7C`).
  This mirrors the original export and keeps every element self‑describing. The colour
  names for those hex values are documented in `assets/tokens.css`.
- **Hover** is done with a `data-hover="…"` attribute — the JS appends that style on
  mouse‑enter and restores it on leave. To change a hover, edit the `data-hover` value.
  *(Elements whose style changes at runtime use CSS `:hover` instead — the `.os-*` and
  `.sec-pill` classes in the `<head>`.)*
- **Responsive** rules live in one place in the `<head>` `<style>` and target the inline
  styles via attribute selectors. Breakpoints: `1024px` (tablet), `768px` / `640px` /
  `600px` (phone).

### Editing the interactive sections

The copy/data for the interactive parts sits in clearly‑named arrays at the top of each
JS block — change these, not the render code:

- **§05 Operating System** — the list rows, layer stack, and expandable text are static
  HTML. Only the *zoom detail panel* is generated from `OS_DETAIL` / `OS_CHIP` in the
  script.
- **§06 In Production** — sector content is in the `SECTORS` object. Only *Medical* and
  *Robotics* carry data; the other pills are inert placeholders.
- **§07 Quote Flow** — the two carousel stages are in the `QST` array (auto‑advances every
  4.2 s; dots jump).
- **§09 Testimonials** — four quote cards written twice (the 2nd set is `aria-hidden`) so
  the CSS marquee loops seamlessly. To add a quote, add it to **both** halves.

---

## Palette (`assets/tokens.css`)

The inline hex values map to these tokens:

| Token         | Hex       | Used for                        |
|---------------|-----------|---------------------------------|
| `--teal`      | `#135B7C` | primary brand / buttons         |
| `--teal-lift` | `#2C86B0` | hover / accents                 |
| `--teal-lite` | `#5FA8CC` | accents on dark                 |
| `--ink-950`   | `#08141B` | deepest bg (CTA / footer)       |
| `--bg`        | `#0C1B24` | dark base bg                    |
| `--ink-800`   | `#102531` | raised dark panels              |
| `--paper`     | `#EFF2F3` | main light bg                   |
| `--text-ink`  | `#13242E` | primary text on light           |
| `--green-deep`| `#3FAE72` | “in production” / floor accents |

Change a colour in `tokens.css` for documentation, then update the matching hex inline.
