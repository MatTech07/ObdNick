<p align="center">
  <img src="app/src/main/res/drawable/logo_obdnick.png" alt="ObdNick Logo" width="220"/>
</p>

<h1 align="center">ObdNick</h1>

<p align="center">
  <strong>Professional OBD-II racing dashboard for Android</strong><br/>
  Real-time engine data · Custom widgets · Bluetooth ELM327 · 0–100 km/h timing
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Kotlin-2.4.10-7F52FF?logo=kotlin&logoColor=white" alt="Kotlin"/>
  <img src="https://img.shields.io/badge/Jetpack%20Compose-Material%203-4285F4?logo=android&logoColor=white" alt="Compose"/>
  <img src="https://img.shields.io/badge/minSdk-24-green" alt="minSdk"/>
  <img src="https://img.shields.io/badge/version-1.0-blue" alt="version"/>
</p>

---


# ObdNick
ObdNick is an OBD-II diagnostic and performance app built for enthusiasts. Customize your dashboard with dynamic gauges, monitor engine telemetry in real-time, and track your 0-100 km/h performance. Experience a zero-lag clean interface with no ads or subscriptions, just pure control over your vehicle’s data.

---

## Table of Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
- [Navigation](#navigation)
- [Dashboard](#dashboard)
- [Performance (0–100 km/h)](#performance-0100-kmh)
- [Live Data](#live-data)
- [Diagnostics](#diagnostics)
- [Bluetooth Connection](#bluetooth-connection)
- [Settings & Themes](#settings--themes)
- [Demo Mode](#demo-mode)
- [Technical Reference](#technical-reference)

---

## Overview

**ObdNick** connects to an ELM327 Bluetooth OBD-II adapter and turns your phone into a configurable racing dashboard. You can monitor RPM, speed, throttle, temperatures, and more through custom widgets, run 0–100 km/h acceleration tests, read trouble codes, and personalize the look with racing-themed color schemes.

The app is designed for low latency, stable Bluetooth sessions, and a clean dark UI optimized for in-car use.

---

## Getting Started

1. **Pair your ELM327 adapter** in Android Bluetooth settings.
2. Open **ObdNick** — the splash screen loads and navigates to the Dashboard.
3. The app **auto-connects** to the last used device (after permissions are granted).
4. When the first engine data arrives, a **Connection Successful** popup confirms the link is live.
5. Use the **☰ menu** (top-left) to switch between screens.

> **Tip:** Enable **Demo Mode** in Settings to explore the app without a car or adapter.

---

## Navigation

| Screen | Description |
|--------|-------------|
| **Dashboard** | Main customizable widget grid with live gauges |
| **Performance** | 0–100 km/h stopwatch and run history |
| **Live Data** | Full list of all supported sensor readings |
| **Diagnostics** | Read and clear OBD trouble codes (DTCs) |
| **Settings** | Theme and Demo Mode |
| **Connect / Disconnect** | Opens the connection screen to scan and pair devices |

The current screen name appears in the top bar. When Demo Mode is active, a dark **Demo Mode Active** badge appears next to the title.

---

## Dashboard

The Dashboard is the heart of ObdNick — a **2-column grid** of widgets that update in real time.

### Widget types

| Type | Description |
|------|-------------|
| **Num** | Large digital readout with unit label |
| **Bar** | Horizontal or vertical bar gauge with dynamic colors |
| **Gauge** | Analog needle gauge (RPM uses a dedicated racing dial) |
| **Arc** | Partial arc gauge with center value |
| **Graph** | Real-time scrolling line chart (last 40 samples) |

Each widget is bound to an **OBD PID** (sensor): RPM, Speed, Throttle, Coolant Temp, Intake Temp, Engine Load, MAF, and others supported by your vehicle.

### Edit mode

Tap the **Edit** FAB (bottom-right) to enter layout editing:

| Action | How |
|--------|-----|
| **Reorder** | Long-press a widget and drag — other widgets slide out of the way |
| **Resize** | Drag the handle at the bottom-right corner (width = half/full row, height = vertical steps) |
| **Configure** | Tap the pencil icon → change PID, widget type, min/max scale, color thresholds |
| **Delete** | Tap the ✕ icon |
| **Add widget** | In edit mode the FAB becomes **+** — pick type and PID |
| **Done** | Tap the **Done** button (bottom-left) to save and resume live polling |

While editing, live data is **frozen** and Bluetooth polling is **paused** to keep the layout stable.

### Color thresholds

In the widget configuration sheet you can define **color ranges** for any value:

- Example: 0–3000 Green, 3000–6000 Yellow, 6000–8000 Red
- Enable **Flash** on a range to make the widget **pulse** when the value enters that zone
- Outside all defined ranges, the widget returns to its **default theme color**

The needle on gauges is **clamped** to the configured min/max arc, but the **digital value always shows the real reading** (even above max).

### Units

Each widget displays its unit next to the numeric value (e.g. `3200 rpm`, `85 km/h`, `92 °C`). Units come from the PID catalog and can be overridden per widget in configuration.

### Default layout

On first launch:

- **Engine RPM** — full-width analog gauge (0–8000)
- **Speed** — digital display
- **Throttle** — arc gauge

Layout, order, sizes, and thresholds are **saved automatically** and restored on restart.

---

## Performance (0–100 km/h)

A dedicated timing screen for measuring acceleration from standstill to 100 km/h.

### How it works

1. Tap **START** — the timer enters *Armed* state and waits for movement.
2. As soon as **speed > 0 km/h**, the timer starts (*Running*).
3. When **speed reaches 100 km/h**, the run completes (*Finished*) and the time is saved.
4. Tap **RESET** to clear and prepare a new run.

### Display

| Area | Content |
|------|---------|
| **Header** | Current speed in km/h |
| **Center** | Live digital timer (seconds, 2 decimal places) |
| **Status** | Idle / Waiting for launch / Running / Run complete |
| **History** | List of past runs with date and time — tap the pencil icon to enter delete mode |

Speed is polled at **maximum frequency** during this screen for the most accurate timing possible.

---

## Live Data

Shows a scrollable list of **all PIDs** currently subscribed and supported by your vehicle. Each row displays the sensor name, formatted value, and unit.

Requires an active Bluetooth connection (Demo Mode does not unlock this screen unless you are also connected).

---

## Diagnostics

Read and manage **Diagnostic Trouble Codes (DTCs)**:

| Button | Action |
|--------|--------|
| **QUICK SCAN** | Fast DTC read |
| **DEEP SCAN** | Extended DTC read |
| **CLEAR MIL CODES** | Clears stored codes (turns off the MIL/check-engine light) — confirmation dialog required |

Requires an active OBD connection.

---

## Bluetooth Connection

### Connection screen (via Connect button)

- View **paired** and **discovered** devices
- Tap a device to connect
- **SCAN** searches for nearby adapters
- **DISCONNECT** ends the session

### Auto-connect

After the splash screen, ObdNick automatically tries to reconnect to the **last successful device**. A banner shows progress while connecting.

### Stability features

- **ELM327 handshake** on every connection (`ATZ`, `ATE0`, `ATL0`, `ATSP0`)
- **Keep-alive** monitors data flow — if no reading arrives for 3 seconds, the socket is probed and reset if needed
- **Foreground service** keeps the Bluetooth link alive while the app is connected
- **Connection popup** appears once when the first real engine PID is received

---

## Settings & Themes

### Themes

Pick an accent color scheme. All themes use a dark racing background:

| Theme | Accent |
|-------|--------|
| Dark | Default |
| Light | Light surface variant |
| Red | Racing red |
| Orange | Sunset orange |
| Green | Emerald green |
| Purple | Midnight purple |

Theme changes animate smoothly across the entire app.

### Demo Mode

Simulates realistic engine data without Bluetooth:

- Toggle **Demo Mode** in Settings
- Resets to **OFF automatically** every time the app is reopened
- While active, a **Demo Mode Active** badge appears in the top bar
- RPM, speed, throttle, load, temperatures, and MAF are generated with physically coherent relationships (speed follows RPM)

---

## Demo Mode

| Feature | Works in Demo? |
|---------|----------------|
| Dashboard widgets | ✅ Yes |
| Performance timer | ✅ Yes (simulated speed) |
| Live Data | ❌ Requires BT connection |
| Diagnostics | ❌ Requires BT connection |
| Auto-connect | Skipped (no adapter needed for dashboard) |

---

<br/>

<details>
<summary><h2>📋 Technical Reference — tap to expand</h2></summary>

### Tech stack

| Layer | Technology |
|-------|------------|
| Language | **Kotlin 2.4.10** |
| UI | **Jetpack Compose** + **Material 3** |
| Architecture | **MVVM** + **Hilt** (dependency injection) |
| Navigation | **Navigation Compose 2.9.8** |
| Persistence | **DataStore Preferences** + **kotlinx.serialization** (JSON) |
| Async | **Kotlin Coroutines** + **StateFlow** / **SharedFlow** |
| OBD protocol | **obd-java-api 1.0** (Mode 01 PIDs, Mode 03/04 DTCs) |
| Drag & drop | **sh.calvin.reorderable 3.1.0** |
| Build | **AGP 9.3.0**, **Gradle 9.5.0**, **KSP**, **compileSdk 37**, **minSdk 24**, **Java 11** |

---

### Project structure

```
app/src/main/java/com/example/obdnick/
├── MainActivity.kt              # NavHost, drawer shell, connection popup
├── CarViewModel.kt              # Facade: BT, polling, DTC, demo state
├── ObdBluetoothManager.kt       # SPP socket, discovery, DTC I/O
├── ObdConnectionService.kt      # Foreground service while connected
├── ObdNickApp.kt                # Application — demo mode reset on launch
├── bluetooth/
│   ├── ConnectivityManager.kt   # Handshake, keep-alive, success events
│   └── AutoConnectManager.kt    # Last-device auto-reconnect
├── data/
│   ├── WidgetConfig.kt          # Widget layout + thresholds model
│   ├── ObdPidCatalog.kt         # PID definitions and decode formulas
│   ├── PerformanceRun.kt        # 0–100 run record
│   └── UserPreferencesRepository.kt  # DataStore (theme, dashboard, runs, demo)
├── obd/
│   ├── PollingManager.kt        # Adaptive / turbo / demo polling engine
│   └── ObdCommandParser.kt      # Raw ELM327 response → ObdReading
└── ui/
    ├── screens/                 # Dashboard, Performance, LiveData, Diagnostics, Splash
    ├── settings/                # SettingsScreen, SettingsViewModel
    ├── components/              # RpmGauge, BarGauge, GaugeMath, ObdCard
    ├── navigation/              # Routes, drawer content
    └── theme/                   # ObdNickTheme, AppTheme enum
```

---

### Adaptive polling (`PollingManager`)

The polling engine uses a **subscriber model**: each screen registers the PIDs it needs. Only subscribed PIDs are queried over Bluetooth.

| Mode | Behavior |
|------|----------|
| **Normal** | Critical PIDs (`0C` RPM, `0D` Speed) every cycle; others every 4th cycle; 25 ms gap between commands |
| **Paused** | Dashboard edit mode — subscriptions kept, no queries sent |
| **Background** | App not in foreground — polling idle (750 ms sleep) |
| **Turbo** | Single PID every **5 ms** — used by Performance screen for speed |
| **Demo** | Simulated data every **100 ms**, no Bluetooth |

Foreground/background is controlled by `MainActivity.onStart` / `onStop`.

---

### Demo data generation

When Demo Mode is active, `PollingManager.demoTick()` produces coherent values:

```
rpmRatio = (rpm − 800) / 6000          clamped 0..1
speed    = rpmRatio × 200              km/h
throttle = rpmRatio × 100              %
load     = throttle × 0.85 + noise     %
intake   = 25 + rpmRatio × 20          °C
coolant  = 82 + rpmRatio × 12          °C
maf      = 2 + rpmRatio × 40           g/s  (consumption proxy)
rpm      = random walk 800..6800       rpm
```

---

### OBD PID formulas (SAE J1979 Mode 01)

| PID | Name | Bytes | Formula | Unit |
|-----|------|-------|---------|------|
| `04` | Engine Load | A | `A × 100 / 255` | % |
| `05` | Coolant Temp | A | `A − 40` | °C |
| `0C` | RPM | A, B | `(A × 256 + B) / 4` | rpm |
| `0D` | Speed | A | `A` | km/h |
| `0F` | Intake Temp | A | `A − 40` | °C |
| `10` | MAF | A, B | `(A × 256 + B) / 100` | g/s |
| `11` | Throttle | A | `A × 100 / 255` | % |

Supported PIDs are discovered at connect time via bitmask queries `0100` and `0120`, filtered against the catalog.

---

### Gauge mathematics (`GaugeMath.kt`)

Shared geometry for all needle and arc gauges:

```
ratio       = (value − min) / (max − min)   → coerceIn(0, 1)
needleAngle = startAngle + ratio × totalSweep
rotation    = needleAngle − 270°            (needle base points up in Canvas)
filledSweep = ratio × totalSweep
```

| Function | Purpose |
|----------|---------|
| `calculateGaugeAngles()` | Single source of truth for arc + needle sync |
| `getRotationAngle()` | Needle rotation applied in Canvas |
| `colorForValue()` | Match value to first threshold range → color; else fallback |
| `shouldFlash()` | True if value is inside a threshold with `flash = true` |
| `GAUGE_REDLINE_FRACTION` | Last 15% of arc/bar drawn as red zone (0.85) |

Digital values are **never clamped** — only the needle/bar fill respects min/max.

---

### Performance state machine

```
IDLE ──[START]──► ARMED ──[speed > 0]──► RUNNING ──[speed ≥ 100]──► FINISHED
                      │                        │
                      └── (already moving) ────┘
Any non-IDLE state ──[RESET]──► IDLE
```

Timer uses `SystemClock.elapsedRealtime()` updated every ~16 ms while running. Completed runs are stored as `PerformanceRun(id, timeMillis, dateEpoch)` in DataStore JSON.

---

### Bluetooth handshake sequence

```
connect(device)
  → openConnection (SPP RFCOMM)
  → ATZ            (reset adapter, wait 1500 ms)
  → ATE0           (echo off)
  → ATL0           (linefeeds off)
  → ATSP0          (auto protocol)
  → discoverSupportedPids()
  → finalizeConnection() + start ObdConnectionService
  → startKeepAlive loop
```

Keep-alive: every 1 s, if `lastReadingTimestamp` is older than 3 s → probe `0100`; on failure → `resetSocket()`.

---

### DataStore keys

| Key | Content |
|-----|---------|
| `app_theme` | Selected `AppTheme` enum name |
| `dashboard_config_json` | `List<WidgetConfig>` as JSON |
| `performance_runs_json` | `List<PerformanceRun>` as JSON |
| `last_device_mac` | Last connected BT MAC address |
| `demo_mode` | Boolean — reset to `false` on every app launch |
| `units` | `"metric"` (reserved for future unit conversion) |

`WidgetConfig` fields: `id`, `sensorKey`, `type`, `span`, `label`, `unit`, `colorHex`, `order`, `minValue`, `maxValue`, `heightSteps`, `thresholds[]`.

`Threshold` fields: `min`, `max`, `colorArgb`, `flash`.

---

### Connection events (single-fire)

| Event | Mechanism | Trigger |
|-------|-----------|---------|
| Connection popup | `SharedFlow<Unit>` replay=0 | First PID received while CONNECTED |
| Demo badge | `StateFlow<Boolean>` from DataStore | Demo Mode toggle |
| Auto-connect banner | `AutoConnectState.CONNECTING` | AutoConnectManager attempt |

---

### Permissions

| Permission | Purpose |
|------------|---------|
| `BLUETOOTH_CONNECT` | Connect to paired adapter (Android 12+) |
| `BLUETOOTH_SCAN` | Device discovery (Android 12+) |
| `ACCESS_FINE_LOCATION` | Discovery on Android ≤ 11 |
| `POST_NOTIFICATIONS` | Foreground service notification (Android 13+) |
| `FOREGROUND_SERVICE_CONNECTED_DEVICE` | Keep-alive service type |

---

### Build

```bash
./gradlew assembleDebug
```

Requires **Android Studio** with JDK 11+ and an ELM327 Bluetooth adapter for live testing.

</details>

---

<p align="center">
  <sub>ObdNick v 1.0 · Built with Kotlin & Jetpack Compose</sub>
</p>

