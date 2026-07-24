# Technology — v2 (proposal)

A rebuild of `../technology.html`, in response to the cofounder's note:
**keep it simple, but high quality.**

`technology.html` in this folder is a drop-in replacement candidate. Nothing
outside this folder was changed except three new files in `../assets/`.

---

## What changed

**Order.** The page now runs in the order the product actually has:

| | v1 (`../technology.html`) | v2 (this folder) |
|---|---|---|
| 1 | Hero — "AI-native manufacturing" | **The Fabricator** — hero *is* the machine |
| 2 | The data thread (SVG pipeline diagram) | **APG** |
| 3 | The five layers (5 render cards) | **FacOS** |
| 4 | APG | **QMS** |
| 5 | FacOS + fake live dashboard mock | **Certifications** |
| 6 | The Fabricator | CTA |
| 7 | QMS | Footer |
| 8–10 | Capability envelope, Network, CTA | — |

**Cut.** Everything that was decoration rather than information:

- the fake drawing-register chrome (`DWG AE-OS-02 · REV C`, sheet/layer plates,
  corner ticks) — it read as costume, not as engineering
- the hand-drawn SVG "data thread" pipeline diagram
- the five layer-render cards (a table of contents for a page with four sections)
- the **hand-built HTML mock of the FacOS dashboard** — ~70 lines of invented
  machine rows, invented OEE numbers and an invented order, carrying a
  "representative for illustration" disclaimer. It is now the real product
  screenshot instead.
- the blueprint grid overlays on four sections
- the capability-envelope and network sections (they belong on Services / Home)
- the full facility-requirements table (compressed air, slab spec) — that is a
  buyer's datasheet, not a web page

Page length went from ~7,800 px to ~4,850 px at desktop width. Nine sections
became four plus a certification band.

**Kept and made the point of the page.** Real media, at full size:

| Section | Media | File |
|---|---|---|
| Fabricator | labelled cutaway **+** cell simulation, toggled | `fabricator-cutaway.png`, `fabricator-simulation.mp4` |
| APG | screen recording of the quoting portal | `APG-quick-video.mp4` |
| FacOS | product screenshot of the live shop-floor view | `facos-dashboard.png` |
| QMS | Zeiss CMM inspection footage | `qms-inspection.mp4` |

---

## The one interaction

Above the hero image: **Cutaway / In motion**.

Both assets were rendered on the Aether paper background (`#EEF0EF` and
`#EFF2F3` — within 1% of each other), at the same isometric angle and the same
scale. So the media well needs no card and no border: the machine sits directly
on the page, and the toggle crossfades between *what the cell is* (labelled,
white, still) and *what it does* (colour, moving). Same machine, same framing,
two answers.

Everything else on the page moves only on scroll — a single rise-and-fade,
staggered, respecting `prefers-reduced-motion`.

---

## Design rules this file follows

- **Light.** `--paper #EFF2F3` and `--card #FFFFFF` alternate. Dark appears
  twice: the FacOS product screenshot (self-contained, inside its frame) and
  the closing CTA + footer.
- **Same tokens, same padding, same type as `../index.html`.** Section padding
  `clamp(72px,9vw,124px) clamp(24px,8vw,120px)`, Montserrat 300/600/700 with
  JetBrains Mono for labels and figures, palette from `../assets/tokens.css`.
- **Inline styles + `data-hover`**, matching the convention in `index.html`.
  Layout primitives that need media queries are the few classes at the top of
  the file (`.sec`, `.split`, `.frame`, `.cell`, `.pt`).
- **No invented data.** Every number on the page (70% OEE, 22 hrs/day, 10,000
  nodes, <15 µm, the cell configuration, the certifications) already appears on
  the approved pages.

---

## Assets added to `../assets/`

| File | Size | Source |
|---|---:|---|
| `fabricator-cutaway.png` | 1.1 MB | `C:\Aether\Media\fabricator-image-.png` |
| `fabricator-simulation.mp4` | 46 MB | `C:\Aether\Media\videos\fabricator-simulation-record01.mp4` |
| `qms-inspection.mp4` | 130 MB | `C:\Aether\Media\videos\Aether-QMS-inspection.mp4` |

Already present and reused: `APG-quick-video.mp4`, `facos-dashboard.png`,
`qms-screenshot.png` (poster frame), `tokens.css`, `../uploads/Primary.png`.

> **Before this ships, the video files need compressing.** `qms-inspection.mp4`
> is 130 MB for 7.8 seconds and `fabricator-simulation.mp4` is 46 MB for 37 s;
> both are 4K/2.5K source files. Re-encoding to 1600 px wide H.264 (plus a WebM)
> should land each under 5 MB with no visible loss at the sizes they are shown.
> The page already ships them as `preload="metadata"`, plays them only while
> they are on screen, and never loads the hidden Fabricator pane until someone
> presses **In motion** — but that is a mitigation, not a fix.
>
> `qms-inspection.mp4` is a 16:9 file with a 2.40:1 image letterboxed inside it.
> The page crops the bars back off with `aspect-ratio:2.4/1` + `object-fit:cover`.
> If the file is re-exported without bars, drop that rule.

---

## Run it

```bash
npx serve C:/GitHub/aether-technology-prototype/aether-home-v2 -l 4173
```

then open `http://localhost:4173/technology-v2/technology.html`.

## To promote it to the live page

Paths in this file point one level up. Two replacements and a move:

1. `../assets/` → `assets/`, `../uploads/` → `uploads/`, `../services.html` →
   `services.html`, `../index.html` → `index.html`
2. move the file to `../technology.html` (replacing v1)
3. `index.html` and `services.html` already link to `technology.html`, so their
   top-bar links need no change

Services has not been touched yet — it should get the same treatment so the two
sub-pages match.
