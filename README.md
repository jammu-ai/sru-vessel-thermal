# Vessel Thermal Mapping Dashboard

An interactive, web-based engineering dashboard for 3D thermal profiling, grid layout planning, and heat distribution analysis on cylindrical pressure vessels and reactor shells.

This application is implemented as a single, self-contained HTML file: [vessel-multi-zone V3.html](file:///C:/progs/SRU%20prog/vessel-multi-zone%20V3.html). It integrates 3D graphics (Three.js), reactive controls (React), and dynamic 2D canvas visualization to provide real-time design feedback.

---

## 🌟 Key Features

1. **3D Interactive Vessel Model**:
   - High-fidelity 3D visualization of the vessel shell, hemispherical heads, saddle supports, and nozzle attachments.
   - Interactive mouse and touch Orbit Controls for rotating, panning, and zooming.
   - Visual references including center axes, circumferential divisions, and clock hour ticks on the heads.
   - Dynamic mapping of measurement zones and thermal datasets directly onto the curved 3D surface.

2. **Multi-Zone Grid Editor**:
   - Modular definition of measuring zones with custom longitudinal positioning (`Start Pos` and `Grid Length`).
   - Interactive angular range selection using a custom circular clock hour selector (e.g., from 10 o'clock to 2 o'clock).
   - Adjustable resolution settings with independent longitudinal (`pitchL`) and transverse (`pitchT`) grid pitches.

3. **Thermal Simulator**:
   - Multi-profile simulated thermal scan generation (Furnace, Cryogenic, Gradient, Uniform, and Custom).
   - Custom configurations for background temperature, random variance, and Gaussian hotspot/coldspot injection.
   - Real-time heat mapping with an 8-stop color gradient scale representing ranges from cold temperatures to extreme heat.

4. **Surface Rollout (2D Unwrapped View)**:
   - Formatted 2D canvas showing the cylinder's surface unwrapped onto a flat grid.
   - Longitudinal position mapped to the X-axis and clock hours mapped to the Y-axis (with 12 o'clock aligned at the center line).
   - Supports zone outlines and real-time interpolated thermal overlays.

5. **Detailed Heatmaps & Measurement Summary**:
   - Separate high-resolution 2D heatmaps showing exact cell coordinates and values for each zone.
   - Complete tabular summary ([GridDetailsTable](file:///C:/progs/SRU%20prog/vessel-multi-zone%20V3.html#L424)) displaying cell counts, surface areas, average/minimum/maximum temperatures, and hotspot occurrences.

---

## 🚀 How to Run

Since the application is fully client-side and compiled on-the-fly, no build step or installation is required:

1. Locate the dashboard file: [vessel-multi-zone V3.html](file:///C:/progs/SRU%20prog/vessel-multi-zone%20V3.html).
2. Double-click or open the file in any modern web browser (e.g., Chrome, Edge, Safari, Firefox).
3. The dashboard will load immediately, rendering the vessel and default zones.

*Note: An active internet connection is required to load the react, three.js, and babel-standalone libraries from their CDNs.*

---

## 🛠 Tech Stack

- **Frontend Core**: React 18 (production build via CDN)
- **3D Rendering**: Three.js r128 (via CDN)
- **Compiling**: Babel-Standalone (for on-the-fly ES6+ and JSX transpiling)
- **2D Rendering**: HTML5 Canvas API
- **Styling**: Vanilla CSS with custom property overrides for sliders and dark theme colors
- **Fonts**: Inter and Monospace
