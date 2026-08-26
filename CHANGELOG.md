# Changelog — Vessel Thermal Mapping Dashboard

All notable changes to `vessel-multi-zone V3.html` are documented here.  
Format: `[vX.Y.Z] — YYYY-MM-DD`  
- **X** = major (architecture / breaking rework)  
- **Y** = minor (new feature or significant enhancement)  
- **Z** = patch (bug fix, small tweak)

---

## [v3.7.1] — 2026-08-26

### Fixed
- **Dish-end grids on the Drawing Sheet's surface development no longer fall outside the dish-end outline.** The heads were drawn as elevation "lens" shapes whose horizontal extent varied with *angular position* (the Y axis), while a grid cell's horizontal position is a function of its *radius* — two different variables, so cells inevitably landed outside the outline and read as sitting behind the dish end. (The quadratic outline also only reached half its nominal crown depth, doubling the overshoot.)
- **Heads are now drawn as a true surface development**: each head unrolls into a band on the same angular Y axis as the shell strip, plotted against meridian arc length measured along the head surface from the tangent line to the crown (Simpson-integrated ellipse-quadrant arc length; flat heads develop 1:1). Cell positions use the band's own scale, so a cell can no longer spill past the outline regardless of clamping. The head grid now reads continuously with the shell's top band across the T.L.
- Major/minor angular gridlines are continued across the head bands, and the crown (apex) edge is marked, so shell and heads read as one development.
- **Corrected a dimensioning error**: the top dimension line spanned the full drawn width (shell + heads) but was labelled OAL. Because the head bands are drawn to *developed* arc length rather than axial depth, that line now dimensions T.L.-to-T.L. only; each head carries its own developed-arc dimension plus crown-depth callout, and OAL is shown as a text note marked not-to-scale. Crown-depth callouts are suppressed on flat heads.

---

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
