# Gerber Package Viewer

A self-contained, single-file HTML Gerber PCB viewer that runs entirely in the browser —
no installation, no server, no build step required.

Load a Gerber ZIP package or individual layer files and instantly see a colour-coded,
interactive rendering of your PCB — including a photorealistic Top/Bottom view with
configurable soldermask colour, copper finish, and silkscreen colour.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Features

| Capability | Detail |
|---|---|
| **ZIP package loading** | Drop or open a Gerber ZIP; all layers are auto-detected and rendered |
| **Single-file loading** | Load any individual `.ger`, `.gbr`, `.gtl`, `.gbl`, `.xln`, `.drl`, etc. |
| **Drag-and-drop** | Drag a file anywhere on the page to load it |
| **Layers / Top / Bottom view** | Toggle between diagnostic layer palette and photorealistic top or bottom view |
| **Realistic PCB rendering** | Soldermask fill, copper traces under glass, bright exposed pads, silkscreen on top |
| **User-configurable colours** | Soldermask presets (Green, Black, Red, Blue, Purple, White) + custom picker; copper finish (ENIG, HASL, ImSn, OSP); silkscreen colour |
| **Region fills (G36/G37)** | Solid copper pour rendering, not just outlines |
| **All aperture types** | Circle (C), Rectangle (R), Polygon (P), Obround/Oval (O) |
| **Excellon drill parsing** | Merged plated + non-plated drill files; correct LZ/TZ handling |
| **Layer visibility toggles** | Show/hide any layer independently |
| **Fit to screen / zoom** | Fit button + ± zoom buttons; 100 % = fit-to-screen |
| **Pan** | Click-and-drag to pan the canvas |
| **Coordinate readout** | Live X / Y position in inches as you move the mouse |
| **Package Info panel** | Board dimensions (in + mm), layer count, bounds |
| **Drill Summary panel** | Total holes, distinct tool sizes, min/avg/max diameter |
| **Fabrication Notes export** | Fill in board specs and export a `Fabrication.txt` for your fab house |
| **No build toolchain** | Uses JSZip (CDN) for ZIP parsing; everything else is vanilla JS + Canvas 2D |

---

## Quick Start

1. Download `gerber_package_viewer_1.0.0.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).
3. Click **Load Gerber File / ZIP** or drag-and-drop your file onto the page.
4. Use **Layers** for a diagnostic view or switch to **Top** / **Bottom** for a
   photorealistic render. Adjust colours in the *Realistic Colors* sidebar card.

> **Offline use:** JSZip is loaded from `cdnjs.cloudflare.com`. For fully offline use,
> download [`jszip.min.js`](https://cdnjs.cloudflare.com/ajax/libs/jszip/3.10.1/jszip.min.js)
> and place it in the same folder as the HTML file, then change the `<script src="...">` tag
> to point to the local copy.

---

## Supported File Formats

### Gerber (RS-274X / Gerber X2)

| Layer | Matched filenames / extensions |
|---|---|
| Board outline / profile | `*.boardoutline.ger`, `*-Edge_Cuts.gbr`, `*.gko`, `*.gm1`, `profile.gbr` |
| Top copper | `*.toplayer.ger`, `*-F_Cu.gbr`, `*.gtl`, `copper_top.gbr` |
| Bottom copper | `*.bottomlayer.ger`, `*-B_Cu.gbr`, `*.gbl`, `copper_bottom.gbr` |
| Top silkscreen | `*.topsilkscreen.ger`, `*-F_SilkS.gbr`, `*.gto`, `silkscreen_top.gbr` |
| Bottom silkscreen | `*.bottomsilkscreen.ger`, `*-B_SilkS.gbr`, `*.gbo`, `silkscreen_bottom.gbr` |
| Top soldermask | `*.topsoldermask.ger`, `*-F_Mask.gbr`, `*.gts`, `soldermask_top.gbr` |
| Bottom soldermask | `*.bottomsoldermask.ger`, `*-B_Mask.gbr`, `*.gbs`, `soldermask_bottom.gbr` |
| Top paste | `*.toppaste.ger`, `*-F_Paste.gbr`, `*.gtp`, `solderpaste_top.gbr` |
| Bottom paste | `*.bottompaste.ger`, `*-B_Paste.gbr`, `*.gbp`, `solderpaste_bottom.gbr` |

Gerber X2 `%TF.FileFunction` attributes are also used as a fallback — any unmatched
`.gbr/.ger` file whose header declares a `FileFunction` will be assigned automatically.

### Excellon Drill

| Format | Notes |
|---|---|
| `.xln`, `.drl` | Auto-detected |
| `.txt` | Loaded only if the file header contains `M48`, `METRIC`, or `INCH` |
| EAGLE `INCH,TZ` | Parsed correctly despite EAGLE writing leading-zero-suppressed coordinates |
| Multiple files | `plated.drills.xln` + `nonplated.drills.xln` merged into one Drill layer |

---

## Layer Colours (Layers view)

| Layer | Colour |
|---|---|
| Copper Top | Orange |
| Copper Bottom | Yellow |
| Board Outline | Light grey |
| Silkscreen Top | Purple |
| Silkscreen Bottom | Magenta |
| Soldermask Top | Green |
| Soldermask Bottom | Bright green |
| Solderpaste Top | Dark gold |
| Solderpaste Bottom | Gold |
| Drill | Cyan |

In **Top / Bottom** view the colours are driven by the *Realistic Colors* settings.

---

## Known Limitations

- **Arc interpolation (G02/G03)** not yet supported — arcs are rendered as straight
  chords; rounded board corners may show small notches in the outline fill.
- **Altium-style filenames** not yet mapped (e.g. `F.Cu.gbr`) — use the Gerber X2
  `%TF.FileFunction` fallback or rename files to match KiCad/EAGLE conventions.
- **Mouse-wheel zoom** not yet implemented — use the **−** / **+** buttons or **Fit**.
- **Aperture macros (`%AM`)** are not rendered (most flashes still appear using the
  fallback aperture shape).

---

## Tested With

- **Autodesk EAGLE 9.7.x** — EAGLE Gerber X2 ZIP export (`.ger` + `.xln`)
- **KiCad 5 / 7 / 8** — standard Gerber export (`.gbr` + `.drl`)

---

## License

MIT License — Copyright (c) 2026 David C. Young.
See the licence comment block at the top of the HTML file for the full text.

---

## Project Structure

```
gerber_package_viewer_1.0.0.html   ← entire application (single file)
CHANGELOG.md                        ← version history
backlog.md                          ← feature & bug tracking
README.md                           ← this file
```

---

## Version History

See [CHANGELOG.md](CHANGELOG.md) for the full history.

| Version | Date | Highlights |
|---|---|---|
| **1.0.0** | 2026-03-24 | First release — MIT licence, realistic colour rendering fix |
| 0.1.0f | 2026-03-23 | Top/Bottom realistic view, configurable colours, O-type apertures |
| 0.1.0e | 2026-03-23 | KiCad ZIP compatibility, negative coordinate fix |
| 0.1.0d | 2026-03-23 | Region fills, Y-axis fix, drill coord fix, zoom fix, layer UI fix |
| 0.0.8 | 2025-10-03 | Extended filename matching, multi-file drill merge |
| 0.0.7 | 2025-06-25 | True-width traces, D03 flashed pads, drag-and-drop |
| 0.0.6 | 2025-06-24 | Layer toggles, coordinate readout, drill summary |
| 0.0.4 | 2025-05-14 | Initial feature set |

---

## Contributing / Development

The entire application is a single HTML file — no build toolchain needed.

1. Open `gerber_package_viewer_1.0.0.html` in your editor.
2. Bump `const VERSION` at the top of the `<script>` block.
3. Add an entry to `CHANGELOG.md` and update `backlog.md`.
4. Test with a known-good Gerber ZIP (EAGLE and KiCad) before committing.
