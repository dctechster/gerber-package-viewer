# Feature & Bug Backlog
📌 Status key: 🟢 Open · 🔵 In Progress · ❌ Broken · ✅ Done

---

## Features

| ID        | Type    | Name / Description                          | Status        | Notes |
|:---------:|---------|---------------------------------------------|:-------------:|-------|
| **F001**  | Feature | Toggle-able `<pre>` Debug View              | 🟢            | |
| **F002**  | Feature | Version auto-increment on export            | 🟢            | Auto-update works at runtime; export script still needs a bump-&-save step |
| **F003**  | Feature | Layer grouping & master toggle              | 🟢            | Basic all-layers checkbox exists; group collapses still TBD |
| **F004**  | Feature | Mouse-wheel zoom & pan                      | 🟢            | Zoom buttons now fully functional (B001 fixed in v0.1.0d); wheel zoom still pending |
| **F005**  | Feature | Coordinate measurement tool                 | 🟢            | Read-out done; distance tool pending |
| **F006**  | Feature | Search & highlight in raw data              | 🟢            | |
| **F007**  | Feature | Custom colour palettes                      | 🟢            | |
| **F008**  | Feature | Drag-and-drop file reload                   | ✅ (v0.0.7)   | |
| **F009**  | Feature | Extended drill filename matching            | ✅ (v0.0.8)   | plated/nonplated .xln, .drl, verbose names |
| **F010**  | Feature | Performance optimisations / lazy parse      | 🟢            | |
| **F011**  | Feature | Altium Gerber compatibility                 | 🟢            | Layer naming, drill formats |
| **F012**  | Feature | Flashed pad (D03) support                   | ✅ (v0.1.0d)  | C, R, and P aperture types; minimum 1 px size enforced |
| **F013**  | Feature | Drill-summary show/hide checkbox            | 🟢            | |
| **F014**  | Feature | Detect & flag un-plated holes               | 🟢            | |
| **F015**  | Feature | Coordinate readout position / follow cursor | ✅ (v0.1.0d)  | Moved to top-left of canvas panel |
| **F016**  | Feature | Layers / Top / Bottom view-mode toggle      | ✅ (v0.1.0f)  | Header pill toggle; Top = top copper/silk/mask, Bottom = mirrored bottom side |
| **F017**  | Feature | Realistic PCB rendering (Top/Bottom modes)  | ✅ (v0.1.0f)  | Soldermask fill, muted traces under mask, bright pads via mask-opening layer |
| **F018**  | Feature | User-configurable realistic colours          | ✅ (v0.1.0f)  | Mask presets + custom picker; ENIG/HASL/ImSn/OSP finish; silk presets + picker |
| **F019**  | Feature | O-type (Obround) aperture rendering          | ✅ (v0.1.0f)  | KiCad elongated SMD pads now drawn as stadium shapes via arcTo |

---

## Bugs

| ID        | Type | Description                                                         | Status        | Notes |
|:---------:|------|---------------------------------------------------------------------|:-------------:|-------|
| **B001**  | Bug  | Zoom % indicator works but canvas does not rescale                  | ✅ (v0.1.0d)  | Fixed: `fitScale` reference added; zoom limits now relative; 100% = fit-to-screen |
| **B002**  | Bug  | Drag-and-drop reload fails                                          | ✅ (v0.0.7)   | Fixed with new overlay handler |
| **B003**  | Bug  | G36/G37 region fills not rendered — copper pours show as outlines only | ✅ (v0.1.0d) | Parser now tracks region paths; filled as polygons with `evenodd` rule |
| **B004**  | Bug  | Y-axis not flipped — board renders upside down                      | ✅ (v0.1.0d)  | `toPX` now inverts Y; drag and coord display corrected |
| **B005**  | Bug  | Excellon TZ/LZ suppression wrong — short coords wildly off          | ✅ (v0.1.0d)  | EAGLE declares `INCH,TZ` but writes LZ; format string parsed; always pad-left |
| **B006**  | Bug  | Sub-pixel silkscreen flashes invisible at fit zoom                  | ✅ (v0.1.0d)  | EAGLE vector-font R-apertures < 0.001 in; minimum 1 px enforced |
| **B007**  | Bug  | Polygon aperture (P type) not rendered                              | ✅ (v0.1.0d)  | `ap.dia` now parsed from `%ADDP` and rendered as circle |
| **B008**  | Bug  | G04 comment lines (e.g. "Gerber X2 export") parsed as X coordinates | ✅ (v0.1.0d) | All G04 lines skipped before coordinate extraction |
| **B009**  | Bug  | Scale display showing 30158% instead of ~100% at fit                | ✅ (v0.1.0d)  | Was `scale × 100` where scale is px/in; now `scale / fitScale × 100` |
| **B010**  | Bug  | Layer checkbox rows misaligned (CSS class conflict with fab panel)  | ✅ (v0.1.0d)  | Layer rows switched to dedicated `.layer-row` class; `for`/`id` wired |
