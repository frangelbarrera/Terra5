# Terra5 -- WORLDVIEW

> NO PLACE LEFT BEHIND

A native macOS geospatial intelligence platform built with SwiftUI and MapKit. Terra5 aggregates multiple real-time data sources -- live flights, satellite orbits, seismic activity, weather radar, and CCTV surveillance -- onto an interactive 3D globe with a tactical HUD interface inspired by military command-and-control systems.

## Origin

This repository is a preserved fork of a now-unavailable project. All source code was written by the original author (with AI-assisted co-authoring on some commits). No code changes have been made from the original. If you are the original author and would like this fork taken down or a license specified, please open an issue.

---

## Features

### Data Layers

| Layer | Source | Refresh | Description |
|-------|--------|---------|-------------|
| Flights | OpenSky Network API | 10 s | Real-time global aircraft positions with altitude bands |
| Satellites | CelesTrak NORAD TLE + SGP4 | 2 s propagation | Live orbital positions, orbit paths, and ground tracks |
| Earthquakes | USGS GeoJSON | 60 s | Past 24 hours, magnitude-scaled markers with colour coding |
| Weather Radar | NOAA NEXRAD | 5 min | Precipitation overlays from 26 radar stations across the US |
| CCTV | Insecam / public feeds | 1 min | Live surveillance camera markers across major cities |

### PANOPTIC AI Detection

Simulated CoreML-based object detection overlay with bounding boxes and corner brackets. Detection categories include Vehicle, Aircraft, Vessel, Building, Person, and Infrastructure. Includes a scanning line animation and real-time detection statistics.

### Visual Modes

Eight post-processing visual modes rendered via MapKit and Cesium:

| Mode | Description |
|------|-------------|
| Normal | Default tactical imagery |
| CRT | Scanline overlay with phosphor tint |
| NVG | Night-vision green phosphor with noise |
| FLIR | Thermal gradient colourmap |
| Anime | Edge detection with flat shading |
| Noir | High-contrast desaturated black and white |
| Snow | Blue-tinted desaturation |
| Panoptic | Detection overlay mode |

### Keyboard Shortcuts

| Shortcut | Action |
|----------|--------|
| Cmd+1-5 | Toggle data layers (Flights, Satellites, Earthquakes, Weather, CCTV) |
| Cmd+Shift+1-8 | Select visual mode |
| Cmd+Option+1-8 | Fly to city preset |
| Cmd+[ | Toggle sidebar |
| Cmd+Shift+P | Toggle PANOPTIC detection |
| Cmd+R | Refresh all data |

---

## Architecture

Terra5 uses a hybrid SwiftUI + MapKit + Cesium architecture:

```
Terra5/
├── App/
│   ├── AppState.swift              # Global state management (@MainActor)
│   └── AppConstants.swift          # Theme, typography, API endpoints
├── Models/
│   ├── CCTVCamera.swift            # Camera model with type/status enums
│   ├── Earthquake.swift            # USGS earthquake model
│   ├── Flight.swift                # OpenSky flight model
│   ├── Satellite.swift             # CelesTrak satellite model
│   └── WeatherRadar.swift          # NOAA radar station model
├── Services/
│   ├── CelesTrakService.swift      # Satellite TLE fetcher
│   ├── DataManager.swift           # Centralised data orchestration
│   ├── InsecamService.swift        # CCTV feed aggregator
│   ├── NOAAService.swift           # Weather radar + alerts
│   ├── OpenSkyService.swift        # Live flight tracker
│   ├── PANOPTICService.swift       # Simulated CoreML detection
│   └── USGSService.swift          # Earthquake monitor
├── Views/
│   ├── Globe/
│   │   ├── CCTVStreamPopup.swift   # Live camera feed popup
│   │   ├── CesiumWebView.swift     # WKWebView Cesium wrapper
│   │   ├── CesiumCoordinator.swift # JavaScript bridge handler
│   │   ├── MapKitGlobeView.swift   # Native MapKit 3D globe
│   │   ├── WeatherLayerPicker.swift
│   │   └── WeatherOverlay.swift
│   └── MainView/
│       └── MainView.swift          # Primary layout container
└── Resources/
    └── Web/cesium/
        └── index.html              # Cesium JS container
```

---

## Requirements

- macOS 13.0+ (Ventura or later)
- Xcode 15+
- Swift 5.9+
- Cesium Ion token (free at [cesium.com](https://cesium.com/ion/tokens)) -- only needed for Cesium 3D globe mode; MapKit mode works without any keys

---

## Getting Started

1. Clone the repository:
   ```bash
   git clone https://github.com/frangelbarrera/Terra5.git
   cd Terra5
   ```

2. Open in Xcode:
   ```bash
   open Terra5.xcodeproj
   ```

3. (Optional) Add your Cesium Ion token in `AppConstants.swift` under `APIEndpoints.cesiumToken`.

4. Build and run (Cmd+R).

---

## Data Sources

All data is sourced from publicly available APIs. No authentication keys are required for basic operation:

- **OpenSky Network** -- Free, unauthenticated access for real-time flight data
- **CelesTrak** -- Public NORAD TLE satellite elements
- **USGS Earthquake Hazards Program** -- Public GeoJSON earthquake feed
- **NOAA National Weather Service** -- Public radar and alerts API
- **Insecam** -- Publicly accessible camera directory

---

## License

No license was specified by the original author.
