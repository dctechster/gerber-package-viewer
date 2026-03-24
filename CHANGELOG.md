# Changelog
All notable changes to *Gerber Package Viewer* are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.0.0] – 2026-03-24

### Added
- **MIT License** — full licence text embedded as an HTML comment block at the top
  of the file; also displayed in the footer.
- **Footer attribution** — footer now reads
  *"Created by David C. Young with AI assistance | MIT License"*.

### Fixed
- **Realistic colour rendering for non-green soldermask** — the muted-copper blend
  used for traces visible through the soldermask glass was 60 % copper + 40 % mask,
  which produced an incorrect brownish board for purple, blue, black, and red masks.
  Formula changed to **65 % mask + 35 % copper** so the mask colour always dominates
  and traces appear as a naturally tinted, slightly lighter version of the mask glass —
  matching how real boards look regardless of soldermask colour.
- **Version strings unified** — `<title>`, `const VERSION`, header badge, footer, and
  `console.info` all now read `1.0.0`.

### Known Issues
- Mouse-wheel zoom not yet implemented (F004).
- Arc / circular interpolation (G02/G03) rendered as chords; board polygon fill may
  show small notches at arc corners until arcs are supported.
- Altium-style Gerber naming not yet mapped (F011).

---

## [0.1.0f] – 2026-03-23

### Added
- **F016 – Layers / Top / Bottom view-mode toggle** — three-button toggle in the header
  switches between *Layers* (diagnostic palette), *Top* (realistic top-side render),
  and *Bottom* (realistic bottom-side render, horizontally mirrored).
- **F017 – Realistic PCB rendering** — Top/Bottom modes render the board as it would
  look in manufacture: board area filled with soldermask colour, copper traces drawn in
  the selected finish colour (muted under mask), soldermask-opening layer drawn in
  bright finish colour (exposes pads), silkscreen in configured colour, drill holes as
  dark circles.
- **F018 – User-configurable realistic colours** — *Realistic Colors* card appears in
  the sidebar when Top/Bottom mode is active. Contains soldermask preset swatches
  (Green, Black, Red, Blue, Purple, White) + HTML colour picker; copper-finish selector
  (ENIG, HASL, ImSn, OSP) with coloured indicator; silkscreen swatches (White, Yellow,
  Black) + colour picker.
- **F019 – O-type (Obround/Oval) aperture rendering** — KiCad elongated SMD pads
  (`%ADD##O,W×H*%`) now render as stadium (rounded-rectangle) shapes using an
  `arcTo`-based path rather than falling through silently.
- **Board polygon fill** — Top/Bottom modes attempt to chain the profile (Edge_Cuts)
  strokes into a closed polygon and fill it with the soldermask colour; falls back to a
  bounds rectangle if the outline cannot be fully chained (e.g. arcs not yet supported).

### Fixed
- **Bottom-view board position** — mirroring now pivots around the board's own centre X
  so the board stays in the same visual position rather than flying off to the right
  canvas edge.
- **Realistic Colors card visibility** — card was hidden in Top/Bottom mode because
  `style.display = ''` removes the inline style but leaves the CSS `display:none` rule
  active; changed to explicit `'block'`.

### Known Issues
- Mouse-wheel zoom not yet implemented (F004).
- Arc / circular interpolation (G02/G03) rendered as chords.
- Altium-style Gerber naming not yet mapped (F011).

---

## [0.1.0e] – 2026-03-23

### Fixed
- **KiCad ZIP compatibility** — all nine layer slots (Cu, Silk, Mask, Paste, Profile)
  now resolved from KiCad-format ZIPs via two mechanisms: (1) KiCad filename patterns
  (`-F_Cu.gbr`, `-B_Cu.gbr`, `-Edge_Cuts.gbr`, etc.) added to every `findFirst` call;
  (2) Gerber X2 `%TF.FileFunction` header fallback scans unmatched `.gbr/.ger` files
  and assigns them by `FileFunction` attribute — works with KiCad, Altium, and any
  X2-compliant tool.
- **Negative coordinate sign handling** — `parseNum` now strips the leading `−/+` sign
  before left-padding the digit string, preventing the sign from consuming an integer
  digit slot (e.g. `-128203006` in 4:6 format → `−128.203006 mm` ✓).
- **`classifySingleFile` extended** — KiCad filename patterns and full X2
  FileFunction matching (Legend, Soldermask, Paste top/bottom) added for single-file
  loads.

### Known Issues
- O-type (Obround) apertures rendered as invisible (fixed in v0.1.0f).

---

## [0.1.0d] – 2026-03-23

### Fixed
- **B003 – G36/G37 region fills** — copper pours were rendered only as hairline
  outlines. Parser now tracks region boundary paths between `G36*` / `G37*` and emits
  a `region` segment type that `drawSegs` fills as a solid polygon using the canvas
  `evenodd` winding rule.
- **B004 – Y-axis flip** — board rendered upside down. Gerber Y increases upward;
  canvas Y increases downward. `toPX()` now maps `canvas.height − (y·scale + offsetY)`.
  Drag handler and coordinate read-out corrected accordingly.
- **B005 – Excellon TZ/LZ suppression** — EAGLE exports declare `INCH,TZ` in the
  header but write leading-zero-suppressed coordinates. Short values such as `Y5000`
  were padded right → `50 000 000` → interpreted as **4 000 inches**. Fix: parse digit
  counts from the format string (`00.0000` → ip = 2, fp = 4) and always pad left.
- **B006 – Sub-pixel silkscreen flashes** — EAGLE vector-font silkscreen uses rectangle
  apertures as small as 0.0008 × 0.0008 in. At fit-to-screen zoom these rendered at
  < 0.3 px and were invisible. Minimum 1 px enforced for all flash dimensions.
- **B007 – Polygon aperture (P type) not rendered** — `%ADDP` apertures had no
  `ap.dia` set and were silently dropped. Outer diameter is now parsed and P-type
  flashes render as circles.
- **B008 – G04 comment lines parsed as coordinates** — `G04 EAGLE Gerber X2 export*`
  contains the token `X2`, which the coordinate regex matched and used to corrupt the
  X register. All `G04` lines are now skipped before coordinate extraction.
- **B009 – Scale display 30158% at fit** — `scale` is stored in px/in (~301 for a
  3 in board). Display is now `Math.round(scale / fitScale × 100)` so Fit = **100 %**,
  and zoom limits are relative to `fitScale`.
- **B010 – Layer checkbox misalignment** — layer rows shared the `.row` CSS class with
  the Fabrication panel input rows. Layer rows now use a dedicated `.layer-row` class;
  `for`/`id` attributes added so clicking a label name toggles its checkbox.
- **F015 – Coordinate read-out position** — box repositioned from bottom-centre to
  top-left of the canvas panel.

### Known Issues
- Mouse-wheel zoom not yet implemented (F004).
- Arc / circular interpolation (G02/G03) not yet rendered; arcs appear as chords.
- Altium-style Gerber naming not yet mapped (F011).

---

## [0.0.8] – 2025-10-03

### Added
- Extended filename matching for alternate exports (`.ger` / `.gbr`; verbose names):
  `boardoutline`, `toplayer` / `bottomlayer`, `topsilkscreen` / `bottomsilkscreen`,
  `topsoldermask` / `bottomsoldermask`, `toppaste` / `bottompaste`.
- Excellon loader now merges multiple drill files: `plated.drills.xln` +
  `nonplated.drills.xln` (+ any `.xln` / `.drl`) into a single Drill layer.
- UI badge next to the Drill layer lists all merged drill filenames.

### Changed
- Raw viewer header now shows layer label + source filename(s).

---

## [0.0.7] – 2025-06-25

### Added
- **Copper fill rendering** — tracks draw at true width; flashed pads (D03) appear as
  filled shapes.
- **Top-right version badge** mirroring the running build (fed by `const VERSION`).
- **Loaded-file label** displayed beside the *Fit* button.
- **Drag-and-drop overlay** works page-wide and hides instantly on drop/leave.
- **Package Info** & **Drill Summary** auto-update after each `renderAll()`.

### Fixed
- **B002 – Drag-and-drop reload** — dropping a ZIP now loads it correctly.

---

## [0.0.6-alpha6] – 2025-06-24

### Added
- F003: All-layer master toggle.
- Drill Summary panel (counts per size in in/mm).
- Package Info auto-stats (layer count, board size).
- Coordinate read-out overlay on the canvas.
- Drag-and-drop ZIP reload workflow.
- Colour-coded layer swatches.
- Fixed footer showing running version.
- Toolbar shows loaded ZIP filename.

---

## [0.0.4] – 2025-05-14

### Added
- F001: Toggle-able `<pre>` Debug View
- F002: Version auto-increment on save
- F003: Layer grouping & "All" toggle
- F004: Zoom & pan canvas
- F005: Coordinate readout & measurement tool
- F006: Search & highlight in raw data
- F007: Custom colour palettes
- F008: Drag-and-drop file reload
- F009: Extended file-name matching
- F010: Performance optimisations
