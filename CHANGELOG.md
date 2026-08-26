# Changelog — Vessel Thermal Mapping Dashboard

All notable changes to `vessel-multi-zone V3.html` are documented here.  
Format: `[vX.Y.Z] — YYYY-MM-DD`  
- **X** = major (architecture / breaking rework)  
- **Y** = minor (new feature or significant enhancement)  
- **Z** = patch (bug fix, small tweak)

---

## [v3.16.0] — 2026-08-26

### Added
- **Accurate 3D Curved Surface Area Calculation for Dished Head Grids** — Added `computeHeadGridArea(end, diameter, zones, idx)` to integrate the 3D surface area cell-by-cell over all active dished head grid tiles ($A_{\text{grid, 3D}} = \sum P_{grid}^2 \sqrt{1 + \frac{H^2 r_c^2}{R^4(1-r_c^2/R^2)}}$). Updated the `Measurement Density` row in `SpecTab` to display the exact 3D grid surface area covered under the measurement tiles alongside plan-view grid area and grid count.

## [v3.15.0] — 2026-08-26

### Added
- **Enriched Spec Table Location & Density Rows** — Restored the classic 15-row General Specification Table (`SpecTab`) parameter layout while integrating full dished head details (Front/Rear head grid tile size, cell counts, surface area $A_{head}$, crown depth $H_f, H_r$, and clock arc range) directly into the `Measurement Location` and `Measurement Density` parameter rows.

## [v3.14.0] — 2026-08-26

### Added
- **3D/Canvas Rollout 12 o'clock Center Alignment** — Updated `Rollout2D` (the canvas 2D rollout view used in the Rollout tab and 3D View side panel) to align 12 o'clock ($0^\circ$, vessel top center) to the **exact vertical center** of the unrolled rollout strip ($Y = yMid$), with 6 o'clock ($180^\circ$) at the top and bottom split seams.
- **Canvas Rollout 1:1 Square Grid Scaling** — Equalized axial and circumferential scale factors in `Rollout2D` so $300\text{mm} \times 300\text{mm}$ grid cells render as **true 1:1 squares** across both shell zones and dished head surface elevation curves.

## [v3.13.0] — 2026-08-26

### Added
- **Uniform 1:1 Aspect Scale Grid Alignment** — Equalized the pixels-per-metre scale factors along both the axial X-axis ($\text{scaleX}$) and circumferential Y-axis ($\text{scaleY}$) in `SurfaceRolloutSchematic` and `Rollout2D`. Square grid cells ($300\text{mm} \times 300\text{mm}$) now render with true 1:1 square proportions across both the unrolled cylindrical shell plate strip and the dished head disc diagrams, eliminating aspect ratio stretching.

## [v3.12.0] — 2026-08-26

### Added
- **Default Dished Head Grid Activation** — Enabled dished head grids by default (`gridEnabled: true`) with $300\text{mm} \times 300\text{mm}$ pitch for Front and Rear dished heads, ensuring measurement grids are automatically displayed across 3D views, 2D rollout schematics, and drawing sheets.
- **Specification Table Dished Head Details** — Updated `SpecTab` (General Specification Table) to auto-calculate and display Front Head ($H_f$), Rear Head ($H_r$), and overall vessel length ($OAL$) details, including shell cells, dished head cells, and total asset sensing points.

### Fixed
- **3D Surface Rollout Grid Mesh Offset** — Optimized `GRID_OFF` offset and line material opacity in `buildVessel`, eliminating Z-fighting and ensuring 3D dished head surface grids sit crisply over the dome meshes.

## [v3.11.0] — 2026-08-26

### Added
- **90° Clockwise Rotated Dished Head 2D Grid Alignment** — Applied a $90^\circ$ Clockwise rotation transform to both Front and Rear Dished Head 2D Disc Grid Diagrams flanking the unrolled shell plate development. The 12 o'clock joining point of the dished head disc now points horizontally to the Right (directly touching the Tangent Line weld seam $T.L.$), aligning the 2D Cartesian measurement grid ($300\text{mm} \times 300\text{mm}$ square tiles) with standard pressure vessel sheet metal surface development guidelines.

## [v3.10.0] — 2026-08-26

### Added
- **Grid-Aligned Dished Head 2D Disc Diagrams** — Positioned Front and Rear Head 2D Disc Grid Diagrams directly beside the unrolled shell plate development (flanking the left and right Tangent Lines). The disc diagrams are rotated and scaled so that the 12 o'clock ($0^\circ$) top center aligns with the middle height of the shell rollout strip, allowing dish-end grid lines ($300\text{mm} \times 300\text{mm}$ square tiles) to match shell zone grid lines 1-to-1 horizontally across the Tangent Lines.

### Fixed
- **Dimension De-Collision** — Re-architected dimension line bands into dedicated non-overlapping horizontal and vertical offsets:
  - Top Band 1 ($OAL$ overall vessel length in blue)
  - Top Band 2 ($H_f, H_r$ front and rear crown depth in red)
  - Left Y-Axis Band ($180^\circ \to 0^\circ \to 180^\circ$ angular degree ticks and `ANGULAR POSITION` title)
  - Bottom X-Axis Band (Axial distance from T.L. in metres)

## [v3.9.0] — 2026-08-26

### Added
- **2-Tier Engineering Surface Development Layout** — Restructured `SurfaceRolloutSchematic` into a formal 2-tier pressure vessel fabrication drawing layout (ASME / ISO standard). Top tier contains the unrolled cylindrical shell plate development ($L \times \pi D$) with elevation head profile outlines, Tangent Line ($T.L.$) markers, overall vessel length ($OAL$), and zone pitch grid lines ($P_L, P_T$). Bottom tier contains dedicated Front and Rear Head 2D Flat Pattern Grid sub-panels with scale disc diagrams, exact 2D square tile grids ($300\text{mm} \times 300\text{mm}$), clock ticks (12, 3, 6, 9 o'clock), pitch, and cell count metadata.

### Fixed
- **Text & Label Overlap Fix** — Increased left axis padding (`leftPad = 80px`) and separated 2D disc diagrams into dedicated sub-panels below the shell rollout, completely resolving text collisions with Y-axis degree ticks ("ANGULAR POSITION", 180° to 180°).

## [v3.8.0] — 2026-08-26

### Added
- **2D Plan-View Dish-End Grid Diagrams** — Added true 2D plan-view disc grid diagrams for Front and Rear dished heads in `SurfaceRolloutSchematic` and `Rollout2D`. Each dish-end grid cell is rendered in its true flat 2D shape (exact $300\text{mm} \times 300\text{mm}$ square tiles) on the disc plane, complete with clock reference ticks (12, 3, 6, 9 o'clock) and thermal heatmap overlays.

### Fixed
- **Surface Development Rollout Figure Update** — Updated the Surface Development schematic layout to present a formal 3-part blueprint figure combining the unrolled shell rectangle with zone grid lines ($Cells_L \times Cells_T$) and flanking 2D dish-end grid disc diagrams alongside Tangent Line ($T.L.$) elevation section callouts.

## [v3.7.2] — 2026-08-26

### Fixed
- **Surface Development & Rollout dished-head geometry fix** — Corrected the dished-head grid transformation in `SurfaceRolloutSchematic` and `Rollout2D` by scaling axial depth `px` relative to the angular dished-head elevation boundary `depthAvail(Y)`. Dish-end grid cells now fit flush inside the dished head elevation curve without spilling into outer margins.
- **Shell Zone measurement grid lines included** — Added explicit grid line rendering ($Cells_L \times Cells_T$) for all shell zones in the Surface Development drawing, ensuring measurement pitch lines ($P_L, P_T$) are drawn in both thermal and non-thermal modes.

## [v3.7.1] — 2026-08-26

### Fixed
- **3D Dish-end grid occlusion fix** — `headSurfacePoint()` now scales depth offset `xo` by radial offset factor `off` and applies normal clearance, preventing dish-end grid lines and thermal surfaces from sinking behind or clipping into opaque vessel heads in 3D viewports (Isometric, Left, Right, Centre, Bottom)
- **Dish-end grid alignment** — `headGridLayout()` now defaults `phaseY` to align grid lines with the top rim of the vessel (`Y = R`), ensuring dish-end grids remain aligned with the top shell grid lines
- **Surface Development & Rollout enhancement** — `SurfaceRolloutSchematic` and `Rollout2D` now properly unroll and render dished-head elevation sections with continuous cell quad-polygons and thermal heatmap overlays, replacing scattered point markers with full surface development

## [v3.7.0] — 2026-08-25

### Added
- **Configurable dish/head ends** — front and rear vessel heads are now independently configurable:
  - Shape type per end: Flat, Hemispherical, or Ellipsoidal (with adjustable depth ratio)
  - Optional thermal measurement grid on each dish surface, independent of the shell's cylindrical zones — modeled as a uniform Cartesian (straight-strip) grid of a single adjustable pitch (default 300mm) draped onto the dome surface, rather than a radial/angular polar grid, so cell size stays consistent instead of converging near the center
  - Each dish-end grid can be restricted to a clock-position arc (same `ClockSelector` control used by shell zones), covering only part of the disc instead of always the full 360°
  - New "Dish Ends" sidebar section with a per-end collapsible editor, matching the existing Zone editor pattern
  - Dish-end grids render as wireframe overlays and, once thermal data is generated, as colored thermal surfaces on the dome geometry; edge cells that only partially overlap the disc are drawn as-is rather than resized to fit exactly
  - New "Dish End Grids" summary table (cell counts, pitch, arc, thermal min/max/avg/hotspots) shown alongside the existing Measurement Grid Details table
  - Fixed a latent bug in the Flat head profile that collapsed every ring to the rim radius (degenerate cap geometry); it now varies correctly from rim to center
  - Export/Import Config extended to persist dish-end shape, grid settings, and thermal data (backwards compatible — older exported files without this field still load with default dish ends)
- **Drawing Sheet tab (`⛁ Drawing Sheet`)** — exportable, A4-landscape general arrangement sheet:
  - Three-row layout of orthographic (true parallel projection) viewports: Isometric + Bottom + Isometric (Rear), Right (rear end) / Centre / Left (front end), and a full-width Surface Rollout panel
  - Isometric view carries arrow-marked zone callouts (leader line + label) pointing at each zone's location on the vessel; a second isometric view shows the opposite end
  - Right and Left views look end-on at the rear and front dish ends respectively, so independently-configured dish shapes are visible from both ends; Bottom is a true plan view looking straight up the vertical axis
  - Removed the redundant Top plan view — that space now goes to a full-width Surface Rollout panel, which reuses the existing 2D unwrapped rollout view (with an unrolled length × circumference dimension caption) plus two new small flat plan-view diagrams of the front and rear dish-end grids (drawn to scale with their own arc-restricted Cartesian cells, or a plain circle when a grid isn't enabled on that end)
  - Toggleable dimension overlay with three levels (Off / Basic / Detailed) — Basic marks overall length, diameter, and each dish end's depth; Detailed adds per-zone axial start/length
  - Reuses the Measurement Grid Details and Dish End Grids tables on the same sheet
  - "Export as Image" button rasterizes the sheet to a downloadable PNG (via html2canvas, newly added as a CDN dependency)
  - Drawing Sheet viewports now render the vessel with near-opaque materials (a new opt-in `printMode`) so the near surface properly occludes the far surface instead of both being visible superimposed — the interactive 3D tab is unaffected and keeps its translucent look
  - Dish-end grid overlays are now visible even before thermal data is generated (a light tint of the end's identity color) and draw with higher-contrast, slightly offset wireframe lines so they read clearly in small end-on views

---

## [v3.6.0] — 2026-07-17

### Added
- **Help tab (`? Help`)** — in-app reference guide covering:
  - Quick Start: 4-step numbered workflow
  - 3D View mouse controls (orbit, zoom, pan, tooltip)
  - All five views explained (3D, Rollout, Heatmaps, Grid Data, Spec)
  - Zone parameter definitions (Start Pos, Grid Length, Clock Arc, Pitch L/T)
  - Thermal Generator controls and profile descriptions
  - Unit system table (SI/MKS, CGS, Imperial, Workshop)
  - Presets reference (what each preset configures)
  - Import/Export behaviour

---

## [v3.5.0] — 2026-07-16

### Added
- **JSON Config Import / Export** — save and reload complete vessel configurations as `.json` files.
  - **⬆ Export Config** button downloads a `vessel-config-YYYY-MM-DD.json` file containing: vessel dimensions, unit system, all zone definitions, thermal generator settings, spec fields, and the current thermal data grid (full snapshot).
  - **⬇ Import** button opens a file picker; loads any exported config and restores all state instantly — including thermal data so the view looks exactly as it was saved.
  - Buttons appear in the Presets section below the three preset buttons (separated by a divider).
  - **Backwards compatible schema** (`schemaVersion: "1"`): every field is optional on import — missing fields fall back to the app's current defaults. Old config files will always be accepted as new fields are added in future versions.
  - Zone `id` values are reassigned on import (runtime-only); thermal data is keyed by zone array index so it survives the reassignment.

---

## [v3.4.5] — 2026-07-16

### Fixed
- **`bootstrap.html` stuck at "Loading…"** — replaced `location.replace(URL.createObjectURL(blob))` with `document.open(); document.write(html); document.close()`.
  - Root cause: `location.replace(blob_url)` changes the page origin to `blob:null`; Chrome blocks `fetch()` from `blob:null` to external HTTPS even with `Access-Control-Allow-Origin: *`, so the launcher's own GitHub fetch silently failed.
  - Fix keeps the `file://` origin intact so the launcher's scripts execute normally and can fetch from `raw.githubusercontent.com`.

---

## [v3.4.4] — 2026-07-16

### Added
- **`bootstrap.html`** — ultra-minimal (~35 lines of logic) file for OTA client distribution.
  - Clients receive this file **once** and never need a new copy again
  - On every open: fetches the latest `Vessel thermal mapping configurator.html` from GitHub, creates a Blob URL, and calls `location.replace()` to seamlessly replace itself with the full launcher
  - The launcher then fetches the latest `vessel-multi-zone V3.html` as before
  - Push any update to GitHub → all clients see it on their next open, zero redistribution
  - Shows XYMA logo + spinner while loading; error state + Retry button if GitHub is unreachable
  - The only hardcoded value is the raw GitHub URL for the launcher (effectively never changes)

---

## [v3.4.3] — 2026-07-16

### Added
- **XYMA Analytics logo** embedded as base64 data URI in both files (no external dependency, works offline):
  - `vessel-multi-zone V3.html` — logo displayed in the tab bar to the left of the version badge
  - `Vessel thermal mapping configurator.html` — logo at the left edge of the launcher header bar

### Changed
- Launcher renamed from `launcher.html` → `Vessel thermal mapping configurator.html`

---

## [v3.4.2] — 2026-07-16

### Fixed
- **Launcher iframe not rendering app correctly** — right-side panel (3D view, tabs) was blank
  - Added explicit `width:100%` and `height:calc(100vh - Npx)` to iframe CSS (fixed positioning alone doesn't stretch iframes like divs)
  - `updateLayout()` now also sets `frame.style.height` dynamically when the update banner shows/hides
  - `updateLayout()` called before `frame.src` is assigned so the app measures correct viewport on first render

---

## [v3.4.1] — 2026-07-16

### Added
- **`launcher.html`** — standalone wrapper page that always fetches and runs the latest version of the app from GitHub.
  - Fetches `vessel-multi-zone V3.html` from `raw.githubusercontent.com` on every open (cache-busted)
  - Parallel GitHub API call to show the last commit date in the header bar
  - Mounts fetched HTML into a full-screen `<iframe>` via Blob URL — app runs in its own context
  - Slim header bar: live version badge, last-updated date, Reload button
  - "Updated v3.x → v3.y" dismissable green banner when a new version is detected (compared via `localStorage`)
  - Loading spinner while fetching; friendly error state if GitHub is unreachable
  - Offline fallback: caches last-fetched HTML in `localStorage`; "Open cached vX.Y.Z" button on error
  - Repo made public so no auth token is required in the page source

---

## [v3.4.0] — 2026-07-16

### Added
- **Manual entry on all sliders** — value display replaced with an inline editable text field. Click to focus, type a value, press Enter or click away to apply. Escape cancels. Value is clamped to slider min/max.
- **Feet + Inches compound input for Imperial** — when unit system is set to *Imperial (ft, °F)*, every length slider shows separate `ft` and `in` fields (0–11 in) instead of decimal feet. Applies to: Shell Length, Shell Diameter, Zone Start Pos, Grid Length, Pitch L, Pitch T.
- **Version constant** (`APP_VERSION`) in script for programmatic access.
- **Version badge** (`v3.4.0`) displayed in the tab bar (top-right), always visible.
- **This CHANGELOG** document created.

---

## [v3.3.0] — 2026-07-16

### Added
- **Multi-system unit support** — dropdown in sidebar (under Vessel) to switch between:
  - SI (m, °C) — default
  - MKS (m, °C) — same as SI
  - CGS (cm, °C)
  - Imperial / USCS (ft, °F)
  - Workshop (mm, °C)
- All displayed values convert live: sliders (min/max/step/value), Grid Details Table headers and cells, Spec tab auto-computed rows, Thermal Legend, Global Min/Max/Range, 3D tooltip, Zone Heatmap 2D axis labels.
- Internal state always stored in SI (m, °C); unit conversion applied only at display layer.

---

## [v3.2.0] — 2026-07-02

### Added
- **Toggle switch** — "Show temperature data in table" under the Thermal Generator section. Thermal columns (Min, Max, Avg °C, Hotspots) hidden by default; revealed on toggle.
- **Asset Name** as first row in Spec tab — manual editable field. The 3D view badge (`👁 ...`) reflects the live value.

### Changed
- Cell count rounding changed from `Math.floor` / `Math.round` → **`Math.ceil`** throughout (grid generation, 3D build, 2D rollout, heatmap, table). Partial cells are always rounded up to match proposal totals.
- Grid area formula changed from geometric arc × length → **cell-based**: `cellsL × pitchL × cellsT × pitchT`. Totals row updated accordingly.
- Thermal data no longer pre-seeded on load; table shows "—" until Generate is clicked.

---

## [v3.1.0] — 2026-06-25

### Added
- **Spec tab** (`◈ Spec`) — General Specification table with auto-computed rows (sensing points, diameter/length, measurement location, density) and editable rows for all other parameters (application, clamping style, MOC, max temp, sensor limits, etc.).
- **Copy to Clipboard** button — writes both `text/html` (Word-compatible table with formatting) and `text/plain` (tab-separated) to the clipboard simultaneously via `ClipboardItem` API.
- **Presets menu** — "Proposal Spec", "Generator Spec", "Multi-Zone Demo" buttons to load pre-configured vessel and zone layouts.
- **Collapsible sidebar** — "◀ Hide / ▶ Show" toggle button in the tab bar.

### Changed
- Full switch to **light theme**: `#f6f8fa` background, `#ffffff` panels, `#0969da` accent, `#1f2328` primary text.
- Fonts upgraded to **IBM Plex Sans** (UI) and **IBM Plex Mono** (values/code) via Google Fonts CDN.
- Three.js renderer clear color updated to `0xf6f8fa`; grid and shell opacity adjusted for light background.
- ClockSelector enlarged (`sz=168`, `r=65`).
- SectionHead redesigned with 3px blue left-bar accent.

---

## [v3.0.0] — 2026-06-18

### Initial V3 Release

**Core features:**
- **3D interactive vessel model** — `THREE.MeshPhysicalMaterial` shell, hemispherical heads, saddle supports, nozzles. Custom `useOrbit` hook for mouse/touch orbit controls.
- **Vertex-colored BufferGeometry** — single draw call per zone for thermal overlay (replaces per-cell mesh approach). Significant WebGL performance improvement.
- **Raycasting tooltip** — `THREE.Raycaster` on mousemove detects zone cells; shows zone name, longitudinal position, clock position, grid cell index, and temperature.
- **2D Surface Rollout** — HTML5 Canvas; cylinder unwrapped with 12 o'clock at center, zones color-coded or thermally overlaid.
- **Zone Heatmap 2D** — per-zone canvas heatmap with positional axis labels.
- **Grid Details Table** — zone-by-zone summary: start, length, arc, pitches, cell counts, area, thermal stats.
- **Multi-Zone Editor** — add/remove zones, each with: Start Pos, Grid Length, arc range (ClockSelector SVG widget), Pitch L/T sliders.
- **Thermal Simulator** — profiles (Normal/Furnace/Cryogenic/Gradient/Uniform); Gaussian hotspot/coldspot injection; 5-point box-blur smoothing pass.
- **Thermal Legend** — 8-stop color gradient from cold (dark blue) to extreme (hot pink/white).
- **Split layout** — 3D view + 2D rollout side by side, Grid Details Table anchored at bottom.

**Tech stack:** React 18 + Three.js r128 + Babel-Standalone (all CDN). Single `.html` file, no build step.

---

## Versioning Policy

| Increment | When to use |
|:----------|:------------|
| **Patch** (Z) | Bug fix, label change, style tweak, no new controls |
| **Minor** (Y) | New feature, new panel/tab, new mode that doesn't break existing configs |
| **Major** (X) | Architecture rework, breaking state changes, rename of the file |

To release a new version:
1. Update `APP_VERSION` constant in the `<script>` block.
2. Update `<title>` tag in `<head>`.
3. Add a new entry to the top of this CHANGELOG (below the `---` separator after the header).
