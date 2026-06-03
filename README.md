# Flood Detection VR

Cross-platform flood monitoring, alerting, and immersive visualization built with React, Three.js, and WebXR.

FloodAlert combines deterministic water-level analytics, real-time sensor integration hooks, and interactive 2D/3D/AR/VR experiences to help users understand flood risk, evacuation thresholds, and the physical scale of rising water for research, education, and emergency preparedness.

## Overview

Flood Detection VR is designed for terrain- and human-scale flood exploration where users need both numerical risk outputs and immersive spatial context.

The platform supports flood workflows using multiple complementary layers:

- **Deterministic risk engine** for water depth, severity tiers, and alert thresholds
- **Interactive React + Three.js visualization** for dashboard, city-scale, and human-scale views
- **WebXR experiences** for field AR and headset VR (Meta Quest and compatible devices)
- **Optional Firebase Realtime Database** integration for live IoT sensor streams
- **TensorFlow.js hooks** for future predictive water-level modeling
- **PWA-ready deployment** for installable browser and GitHub Pages workflows

## Key Features

- Real-time flood depth calculation from sensor level vs. ground elevation
- Multi-tier alerting: NONE, MODERATE, HIGH, and CRITICAL severity states
- Animated GLSL water shader with physics-inspired wave motion
- Human-scale submergence visualization with animated character model
- **Dashboard** — 2D controls, live readouts, and 3D preview with orbit camera
- **Field AR** — WebXR augmented reality water plane anchored to the real world
- **War Room VR** — city digital twin with patrol drones, buildings, and live status panels
- **Ground Zero VR** — first-person ground-level flood immersion with cinematic post-processing
- **Safe Room VR** — indoor control-room scenario with emergency monitor and glass-wall view
- Firebase subscription helper for live sensor data (`subscribeToSensor`)
- Mapbox terrain-RGB elevation utility for future geospatial integration
- Progressive Web App manifest and offline-ready build pipeline

## Tech Stack

| Component | Technology |
|-----------|------------|
| Frontend Framework | React 19 + Vite 8 |
| Frontend Language | JavaScript (ES Modules) |
| 3D Visualization | Three.js, @react-three/fiber, @react-three/drei |
| XR / AR / VR | @react-three/xr (WebXR) |
| Post-Processing | @react-three/postprocessing |
| State Management | React `useState` (Zustand available in dependencies) |
| Real-Time Data | Firebase Realtime Database |
| Predictive AI (planned) | TensorFlow.js |
| Styling | Tailwind CSS, PostCSS |
| PWA | vite-plugin-pwa |
| Tooling | ESLint |

## Architecture and Project Structure

The project is a single-page React application with modular WebXR scenes and shared flood logic:

```
flood-detection-vr-main/
├── src/
│   ├── components/
│   │   ├── webxr/              # AR and VR scene experiences
│   │   │   ├── ARScene.jsx     # Field augmented reality mode
│   │   │   ├── DigitalTwin.jsx # War Room VR city overview
│   │   │   ├── GroundZero.jsx  # First-person ground-level VR
│   │   │   └── SafeRoomVR.jsx  # Indoor safe-room VR scenario
│   │   ├── shaders/
│   │   │   └── WaterPlane.jsx  # Custom GLSL animated water surface
│   │   └── HumanModel.jsx      # Animated human submergence reference
│   ├── lib/
│   │   ├── waterLogic.js       # Depth, severity, and elevation utilities
│   │   ├── firebase.js         # Realtime Database sensor subscriptions
│   │   └── aiModel.js          # TensorFlow.js prediction placeholders
│   ├── assets/                 # Static UI assets
│   ├── App.jsx                 # Main app shell and view routing
│   ├── main.jsx                # React entrypoint
│   └── index.css               # Global styles
├── public/                     # Favicon, icons, PWA assets
├── vite.config.js              # Vite build/dev and PWA configuration
├── tailwind.config.js          # Tailwind theme and content paths
├── postcss.config.js           # PostCSS pipeline
├── eslint.config.js            # ESLint rules
├── package.json                # Scripts and dependencies
└── README.md
```

## Installation

### Prerequisites

- **Node.js 18+** recommended
- **npm** available in PATH
- **WebXR-capable browser or headset** for AR/VR modes (Chrome/Edge on Android for AR; Meta Quest Browser or PC VR for VR)
- **(Optional)** Firebase project for live sensor streaming
- **(Optional)** HTTPS-served dev host for WebXR on local network devices

### Steps

1. Clone or download the repository:

```bash
git clone <your-repository-url>
cd "Flood Detection VR/flood-detection-vr-main"
```

2. Install dependencies:

```bash
npm install
```

3. Start the development server:

```bash
npm run dev
```

4. Open the app:

- **Frontend:** http://localhost:5173

### Run on Specific Workflows

| Workflow | Command |
|----------|---------|
| Local development | `npm run dev` |
| Production build | `npm run build` |
| Preview production build | `npm run preview` |
| Lint | `npm run lint` |
| Deploy to GitHub Pages | Update `homepage` in `package.json`, then `npm run deploy` |

## How to Use

1. Launch the dev server with `npm run dev`.
2. Open the app in a modern browser.
3. Use the header to switch between **Dashboard**, **Field AR**, **War Room VR**, **Ground Zero VR**, and **Safe Room VR**.
4. On the Dashboard, adjust water level with **Increase** / **Decrease** controls to simulate sensor readings.
5. Watch alert severity, evacuation warnings, and the 3D human submergence model update in real time.
6. For AR/VR modes, click **ENTER AR** or **ENTER VR** on a compatible device.
7. (Optional) Configure Firebase in `src/lib/firebase.js` and wire sensor subscriptions into the Dashboard state.

### Alert Thresholds

Water depth is computed as `sensorLevel - groundElevation`. Severity tiers:

| Depth | Severity |
|-------|----------|
| ≤ 0 m | NONE — system normal |
| > 0 m | MODERATE |
| > 0.5 m | HIGH |
| > 2.0 m | CRITICAL |

The Dashboard triggers an evacuation warning when depth exceeds ~1.4 m (approximate chest height on the human model).

## Configuration

### Firebase (Optional)

Replace the placeholder values in `src/lib/firebase.js` with your Firebase project credentials, then subscribe to a sensor:

```javascript
import { subscribeToSensor } from './lib/firebase';

subscribeToSensor('sensor-001', (data) => {
  // data.level — use as sensorFloodLevel in calculateSubmergenceRisk()
});
```

### GitHub Pages Deployment

Update the `homepage` field in `package.json` to match your GitHub Pages URL before running:

```bash
npm run deploy
```

### PWA

The app registers as **FloodAlert** via `vite-plugin-pwa` in `vite.config.js`. Add `pwa-192x192.png` and `pwa-512x512.png` to `public/` for full install icon support.

## Assets and Data Requirements

Ensure these areas are present for your workflow:

| Path | Purpose |
|------|---------|
| `public/` | Favicon, PWA icons, static assets |
| `src/assets/` | Bundled UI assets |
| Firebase Realtime Database | Live IoT sensor readings (optional) |

If Firebase or deployment paths change, update `src/lib/firebase.js` and `package.json` (`homepage`) accordingly.

## Development Notes

- Main app entry: `src/App.jsx`
- Flood risk logic: `src/lib/waterLogic.js`
- WebXR scenes: `src/components/webxr/`
- Water shader: `src/components/shaders/WaterPlane.jsx`
- Build/dev config: `vite.config.js`
- WebXR requires a secure context (HTTPS) on physical devices; use `npm run preview` behind HTTPS or deploy to a hosted environment for headset testing.

## Current Status

Flood Detection VR is a functional prototype with broad coverage for flood visualization, alerting, and immersive WebXR scenarios.

Further improvements can focus on:

- wiring Firebase sensor streams into the Dashboard by default,
- loading a trained TensorFlow.js model in `aiModel.js` for predictive alerts,
- integrating Mapbox terrain-RGB elevation for location-aware ground levels,
- stronger automated test coverage for `waterLogic.js` and UI states,
- HTTPS dev tooling and CI/CD for build, lint, and GitHub Pages deploy paths,
- expanded LiDAR/DEM or GIS data pipelines for site-specific digital twins.
