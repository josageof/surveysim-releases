# SurveySim — MBES Survey Simulator

A desktop survey simulator that generates realistic multibeam echo-sounder data, vessel motion, and sensor outputs for training, testing, and software development.

---

## Installation

Download the latest installer (`SurveySim.Setup.x.y.z.exe`) from the [Releases page](https://github.com/josageof/surveysim-releases/releases) and run it.
After installation, launch SurveySim from the Start Menu or desktop shortcut.

---

## Quick Start

1. **Launch SurveySim** and sign in (or use a cached offline session).
2. **Set up the CRS** (CRS tab) — select UTM zone and hemisphere matching your survey area.
3. **Configure the vessel** (VEH tab) — adjust draft, sensor offsets from CRP, and model opacity.
4. **Set the start position and speed** (NAV tab → Mission Setup).
5. **Choose a navigation mode** (NAV tab → Navigation Mode):
   - **Manual** — control heading with the slider.
   - **Waypoint** — click the map or enter lat/lon; vessel steers automatically.
   - **Runline** — upload a CSV of UTM waypoints; vessel follows the planned line.
6. **Configure the environment** (ENV tab) — set wave height, period, direction, and enable/disable heave/surge/sway/roll/pitch.
7. **Configure MBES** (MBES tab) — set number of beams, swath angle, SVP mode, and recording.
8. **Connect sensor outputs** (OUT tab) — configure TCP/UDP/Serial for GPS, Gyro, IMU, and MBES outputs.
9. **Press Start** — simulation begins. Watch the vessel in the 3D navigation view.
10. **Press Stop** to pause, or use **Return to Start** to teleport back to the initial position.

---

## Interface Overview

### Tabs (left panel)

| Tab | Purpose |
|-----|---------|
| **CRS** | Coordinate Reference System — UTM zone, hemisphere, EPSG |
| **ENV** | Environment — wave model (height, period, direction), motion scaling |
| **VEH** | Vessel model — draft, opacity, CRP and sensor offsets (X/Y/Z from CRP), sensor colors |
| **NAV** | Navigation — start position, speed, autopilot, navigation mode (manual / waypoint / runline) |
| **LOG** | Navigation log — enable, output folder, interval, auto-rotation |
| **MBES** | Sonar — beams, swath angle, SVP, recording, import/export point cloud |


### 3D View (right panel)

- **VESSEL** sub-tab: 3D model with sensor spheres.
  - Buttons on the left: **3D** (free orbit), **TOP**, **BTM**, **BOW**, **STN**, **STBD**, **PORT** — planar views lock rotation; only pan and zoom remain active.
- **NAVIGATION** sub-tab: Globe with live vessel position, runline, waypoint indicator, and MBES point cloud.
  - **[T]** / camera button: cycles Free → Lazy → Track camera modes.
  - **[M]** / mode button: toggles 2D ↔ 3D scene.
  - **MBES** button: toggles point cloud colorization between depth (rainbow) and backscatter (grayscale).

---

## Sensor Outputs (NMEA over TCP/UDP/Serial)

| Sensor | Protocol | Default port | Sentences |
|--------|---------|-------------|-----------|
| GPS1 | UDP | 50001 | GGA, RMC, VTG |
| GPS2 | UDP | 50002 | GGA, RMC |
| GYRO1 | UDP | 50003 | HDT |
| IMU | UDP | 50004 | ROT, custom pitch/roll/heave |
| MBES | — | — | Coming feature |

---

## MBES Recording

- Enable recording in the **MBES** tab before pressing Start.
- Data is saved as a binary `.bin` file in `%APPDATA%\SurveySim\cache\`.
- Use **Export LAS** to export the accumulated point cloud as a `.las` file.
- Use **Import .bin** to reload a previously recorded session and visualise it in the Cesium view.

---

## Navigation Log

- Enable in the **LOG** tab; configure interval and auto-rotation.
- Logs are saved as `.csv` files (lat, lon, heading, pitch, roll, heave, time) in the configured folder (default: `%APPDATA%\SurveySim\logs\`).

---

## Settings Persistence

All configuration (CRS, vessel, MBES, environment, outputs, logs) is saved automatically to:
```
%APPDATA%\SurveySim\settings.json
```
