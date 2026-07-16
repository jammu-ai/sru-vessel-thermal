# Proposed Changes & Feature Enhancements

This document outlines the proposed updates and required changes for [vessel-multi-zone V3.html](file:///C:/progs/SRU%20prog/vessel-multi-zone%20V3.html) to improve performance, user experience, and features.

---

I am using this program to make details for making a commercial proposal.

### 📋 Commercial Proposal Specifications (from [image.png](file:///C:/progs/SRU%20prog/image.png))

| Parameter | Specifications / Requirements |
| :--- | :--- |
| **Application** | Multi-Point Real-Time Continuous Outer Skin Temperature Monitoring using Ultrasonic waveguide for REDUCING GAS GENERATOR |
| **Number of sensing points** | 250 |
| **Number of sensing elements** | As required |
| **Clamping Style** | Steel rope, strap on |
| **Shell diameter, Length** | 2200mm, 6600mm |
| **Measurement Location** | 10 o'clock and 2 o'clock (120° topside between 300° to 60°) |
| **Measurement Density** | **Zone 2:** Insight of every 0.25m² (500x500mm) grid for the 11 meters. Total number of grids: 154. Total Area to cover: ~39.17m². Min Number of grids: 154 Nos. |
| **Shell MOC** | SA-516 Gr. 70N NACE (**Ferromagnetic**) |
| **Max skin temperature** | 345°C |
| **Sensor limits** | Sensor shall withstand temperature upto 1050°C |
| **Flue gas composition** | -NA- |
| **GA drawing provided** | Yes |
| **Communication** | Modbus / IIoT |
| **AI Application** | Needed, Thermal Mapping and AI predictions |

*Note: (To be finalised during detailed engineering)*

---

### Checklists
- [x] **Proposal Specification Table**: Transcribed all parameters from [image.png](file:///C:/progs/SRU%20prog/image.png) into the table above.
- [x] **Match default UI layout to [image-1.png](file:///C:/progs/SRU%20prog/image-1.png)**:
  - Configured the default state in [vessel-multi-zone V3.html](file:///C:/progs/SRU%20prog/vessel-multi-zone%20V3.html) to render a 12m vessel, 3.4m diameter, and Zone 1 starting at 0.5m with a length of 10.5m, 120° topside arc (10 o'clock to 2 o'clock), and 0.5m grid pitches (yielding exactly 147 grids and 37.38 m²).
  - Added a **Presets** menu in the sidebar to toggle between:
    - **Proposal Spec**: The exact setup in [image-1.png](file:///C:/progs/SRU%20prog/image-1.png).
    - **Generator Spec**: Setup reflecting the 2.2m diameter and 6.6m shell length with 11m overall measurement grids.
    - **Multi-Zone Demo**: The default layout with two zones and finer pitches.

## 📋 Summary of Proposed Changes

| Feature / Bug | Category | Description | Priority | Status |
| :--- | :--- | :--- | :--- | :--- |
| **3D Rendering Optimization** | Performance | Consolidate individual cell meshes into a single vertex-colored `BufferGeometry` to reduce WebGL draw calls and prevent browser lag on fine grid resolutions. | 🔥 High | **Implemented** |
| **Interactive Tooltips (Raycasting)** | UX / UI | Add a 3D raycaster to detect mouse hovers over cells, showing tooltip metadata (Zone, coordinates, clock hour position, temperature). | ⚡ Medium | **Implemented** |
| **Split View Layout (image-1.png)** | UI / Layout | Display the interactive 3D model on the left, the 2D surface rollout on the right, and the measurement grid summary table at the bottom. | 🔥 High | **Implemented** |
| **Collapsible Sidebar Panel** | UX / Presentation | Added a button on the tab bar to slide open/close the left editor panel to enable fullscreen presentation view. | ⚡ Medium | **Implemented** |
| **Data Import/Export (JSON)** | Functionality | Allow users to download current configurations and simulated thermal scans as JSON files, and reload them. | ⚡ Medium | Pending |
| **Nozzle & Saddle Configurator** | Control | Move hardcoded nozzle/saddle properties to editable controls in the left panel. | 🔹 Low | Pending |
| **2D-to-3D View Linking** | UX | Clicking on a zone cell in the Rollout 2D or Heatmap view highlights and rotates the 3D camera to focus on that specific location. | 🔹 Low | Pending |

---

## 🔍 Detailed Implementation Plans

### 1. 3D Rendering Optimization
- **Current Approach**: Iterates over every single grid cell $(r, c)$ in [buildVessel](file:///C:/progs/SRU%20prog/vessel-multi-zone%20V3.html#L144), creating a new `THREE.BufferGeometry` and `THREE.MeshBasicMaterial` per cell. For small grid pitches, this can generate $3000+$ meshes, degrading frame rates.
- **Proposed Approach**:
  - Build a single `THREE.BufferGeometry` per zone (or for the entire vessel's thermal cells).
  - Define vertex positions, indices, and interpolate colors (`color` attribute) based on the thermal mapping.
  - Render using a single `THREE.MeshBasicMaterial` with `vertexColors: true`.
  - This reduces draw calls from thousands to just one per zone.

### 2. Interactive Tooltips (Raycasting)
- **Proposed Approach**:
  - Set up a `THREE.Raycaster` in [ThreeCanvas](file:///C:/progs/SRU%20prog/vessel-multi-zone%20V3.html#L234) linked to `mousemove` events on the canvas.
  - Render an HTML tooltip div positioned absolute relative to the cursor.
  - When hovering over a zone cell, query the intersection point, compute the cylindrical projection to map it back to $(r, c)$ grid coordinates, and read the local temperature value to display.

### 3. Data Import/Export (JSON)
- **Proposed Approach**:
  - Add a **JSON Export/Import** block in the left panel's "Thermal Generator" or "Vessel" section.
  - **Export**: Generate a data URI from the `zones` array and `thermalDataMap`, downloading it as a `.json` file.
  - **Import**: Provide a file input listener that parses the JSON, validates dimensions, and sets the state for `length`, `diameter`, `zones`, and `thermalDataMap`.

---

## 🚀 Feedback & Approval
Please select which modifications you would like to apply, or let me know if you have additional changes to include in this log. Once aligned, we can apply the updates directly to [vessel-multi-zone V3.html](file:///C:/progs/SRU%20prog/vessel-multi-zone%20V3.html).
