# Changelog — Vessel Thermal Mapping Dashboard

All notable changes to `vessel-multi-zone V3.html` are documented here.  
Format: `[vX.Y.Z] — YYYY-MM-DD`  
- **X** = major (architecture / breaking rework)  
- **Y** = minor (new feature or significant enhancement)  
- **Z** = patch (bug fix, small tweak)

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
