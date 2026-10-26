# SurveySim — Marine Survey Simulator

A desktop simulator of a hydrographic survey. It reproduces the vessel, the sensors, the water column, the seabed and the data of real multibeam equipment, so that you can **train**, **test your acquisition and processing software** and **calibrate a system** the way you would at sea — without leaving the office.

- A moving vessel in a real place, with irregular sea, hull response, tide and a full vertical reference.
- GNSS, gyro, motion sensor and sound velocity sensors with datasheet errors, sending the telegrams of the real equipment.
- A multibeam (Kongsberg EM, Teledyne Reson, Norbit, R2Sonic) that traces rays through a true water column and sends its data in the real formats.
- Raw sonar files (`.s7k`, `.kmall`) to process in Qimera, MB-System, CARIS and others.
- A patch test training module, with a hidden misalignment of the transducer.

---

## Installation

Download the latest installer (`SurveySim.Setup.x.y.z.exe`) from the [Releases page](https://github.com/josageof/surveysim-releases/releases) and run it. You can install over a previous version.
After the installation, start SurveySim from the Start Menu or the desktop shortcut.

**Requirements:** Windows 10 or 11 (64-bit). Internet is needed to sign in and renew the license, and to download the map imagery and the bathymetry of a new area the first time (they are cached afterwards).

### Sign-in and license

- Create an account in the sign-in window and **confirm your email** (open the link we send you), then sign in.
- **Forgot password** is inside the app: a code is sent by email and you are signed in with the new password.
- The license is a signed lease tied to your computer. It works **offline for up to 7 days** and is renewed automatically, every 60 minutes, while you are online.
- **About** (the *i* button below SYSTEM) shows your version, account, plan and license validity.

---

## Quick start

1. **Sign in** and open the app. Use **File → Open** for a project (`.sim`) or start a new one.
2. **GLOBAL → GEODESY**: set the project CRS (UTM zone or EPSG). Use the same CRS in your acquisition and processing software. **DATUM** and **TIDE** set the vertical reference.
3. **VEHICLE**: pick the vessel and check the sensor offsets from the CRP.
4. **SENSOR**: choose the equipment — GNSS, GYRO, MOTION, SVS / SVP and MBES (model, frequency, swath, stabilization).
5. **MISSION → NAVIGATION**: set the start position and speed, choose the mode — **Manual**, **Waypoint** or **Runline** (line plan) — and use **Start** in the RUNTIME panel.
6. **DATA → OUTPUT**: send each sensor to your software (UDP, TCP, serial or the data feed), then connect.
7. **RUNTIME → SVP**: cast the sound velocity profiler and load the cast into the sonar processor before pinging.
8. **RUNTIME → MBES**: **Start pinging**. Watch the soundings in the NAVIGATION and SURVEYING views.
9. **File → Save** keeps the whole survey in one `.sim` file.

---

## Interface

### CONTROLS (left panel — icons on the left, same order as the tabs)

| Group | Tabs | Purpose |
|-------|------|---------|
| **GLOBAL** | GEODESY · DATUM · TIDE · SEA STATE · WATER | Project CRS, geoid and vertical reference, tide, waves and swell, true water column |
| **SEABED** | BOTTOM · OBSTACLES | Seabed type and noise; wrecks and structures as 3D models (`.glb`) with burial |
| **VEHICLE** | one tab per vessel | Offsets, sensor positions, hull, minimum under-keel clearance |
| **SENSOR** | GNSS · GYRO · MOTION · SVS / SVP · MBES | Equipment catalog and settings |
| **CALIBRATION** | PATCH TEST | Patch test training: site and lines, hidden misalignment and results |
| **MISSION** | PLANNING · NAVIGATION · SURVEYING | Line plan and runline library; mission setup, autopilot and navigation mode; the SVP in the sonar processor, the IHO standard and the swath display |
| **DATA** | OUTPUT · LOGGING · EXPORT | Sensor outputs, navigation log and raw sonar logging, import/export of soundings |
| **SYSTEM** | MONITOR · COLOR | System health thresholds and the global color palette |

### 3D VIEW (right panel)

- **VEHICLE** — the vessel model with the sensor positions; planar views (TOP, BOW, STBD…) and free orbit.
- **NAVIGATION** — map with the live position, the line plan and the online DTM (the mean depth of each cell, like the DTM of the navigation software). Cameras: Free / Lazy / Track.
- **SURVEYING** — the soundings as a point cloud with the swath, the SVP cast going down and the colour scale of your palette.

### RUNTIME (bottom left) and dashboards (bottom right)

- **NAVIGATION** — Start / Pause / Reset of the mission. Reset stops the mission and takes the vessel back to the start; it never changes your settings.
- **SVP** — cast the profiler, follow the descent, compare the SVS with the loaded profile and load the new cast.
- **MBES** — **Start pinging**, **Clear**, **Auto SOL/EOL** (the sonar pings only from the start to the end of each line of the plan, no run-in or run-out), **Split file** and the live sounding statistics.
- Dashboards: NAV · GNSS · GYRO · MOTION · MBES · VESSEL.

---

## Survey lines

- **Line plan** (MISSION → PLANNING): several runlines in sequence, each with its own run-in, run-out, direction and speed. Runlines can be added from files (CSV with `lat,lon` or `x,y`; EIVA `.rlxp`), drawn on the map or taken from the **runline library** — a watched folder, such as the Runlines folder of Helmsman / NaviPac.
- The coordinates are E/N of the project CRS, which must be the same in the other software.
- **Set to Start** takes the vessel to the start of the mission of the current mode; **Start** continues from where the vessel is.

---

## Sensor outputs

Each sensor is sent in **DATA → OUTPUT**, to one or more destinations (UDP, TCP, serial, or the data feed to an acquisition computer).

| Sensor | Telegrams |
|--------|-----------|
| GNSS (2 receivers) | NMEA GGA, GST and ZDA, plus VTG or GGK depending on the receiver model |
| Gyro | NMEA heading (HDT) with the update rate and noise of the model |
| Motion sensor | TSS1, EM3000, KM Binary or PSXN attitude, by model; integrated INS (Seapath 385-5+) |
| SVS | Valeport miniSVS and AML formats |
| Multibeam | Teledyne 7k (e.g. record 7027) · R2Sonic BTH0 · Kongsberg KMALL `#MRZ`, by model |

The simulator does not tell the acquisition chain about the hidden errors of the training modules: the telegrams carry what a real sensor would send.

---

## Raw sonar logging (DATA → LOGGING)

The simulator records the raw data as the sonar's own software does, to process the survey without the acquisition software:

| Sonar | File |
|-------|------|
| Teledyne Reson SeaBat 7125, Norbit iWBMSh STX | `.s7k` (header and catalog like 7k Center, installation, SVP, GNSS antenna, attitude and every ping) |
| Kongsberg EM 2040 | `.kmall` (installation, SVP, position, attitude and every ping) |

- **One file per survey line**: **Start pinging** opens a file and **Stop pinging** closes it; **Split file** (RUNTIME → MBES) starts the next one without stopping. With **Auto SOL/EOL** the lines are separated for you.
- R2Sonic and the generic multibeam do not record raw files: their data is recorded by the acquisition software, as in a real installation.
- Close the app normally (File → Exit or the window button) so that the file in use is completed.

---

## Patch test training (CALIBRATION → PATCH TEST)

1. **1 · SITE AND LINES** — pick a target (for example a wreck) and create the five calibration lines: pitch/roll, latency and yaw. The transducer, not the vessel reference point, passes over the target. The **site check** shows which misalignment that site can resolve — a good site is an isolated target with steep faces on a flat seabed, in 50–70 m of water.
2. **2 · MISALIGNMENT AND RESULTS** — switch on the parameters (roll, pitch, yaw, latency), survey the lines, compute the patch test in your software and type your values; **Check** compares them with the hidden misalignment and **Reveal** shows it. **Tolerances for an IHO S-44 order** can be suggested for the depth and speed of the lines.

Latency is a sonar clock offset: it delays the ping in relation to both the position and the motion sensor. Processing software solves the position latency only — apply the value you find to the motion sensor too, or survey the patch test with latency off.

---

## Recording and exporting soundings

- **RUNTIME → MBES** records the soundings in a `.bin` file while pinging, one per line (**Open folder** shows it in Windows Explorer). The files are in `%APPDATA%\SurveySim\cache\`.
- **DATA → EXPORT**: **Export LAS** exports the accumulated point cloud; **Import .bin** reloads a recorded session.
- **DATA → LOGGING → NAV LOG** saves the vessel navigation to `navlog_<date>_<time>.csv` files, in the folder you choose.

---

## Projects and where the files are

- A project (`.sim`) holds the whole survey: settings, line plan, seabed objects with their models, sound velocity casts, soundings and the outputs. Double-click a `.sim` file to open it in SurveySim.
- Unsaved changes are checked when you close, and a recovery copy is kept if the computer is switched off while you work.
- `%APPDATA%\SurveySim\` keeps only the application data: recent projects, the recovery copy, the cache (soundings `.bin`, bathymetry), the cast files and the logs.
- **File → New project** starts from the factory settings.

---

## Notes

- BOSS and the OPERATION (ROV) tab are not part of this version.
- With every misalignment corrected, a few centimetres of ripple remain at the outer beams: the motion sensor and sounding errors, as in a real survey.
