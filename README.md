# 🚀 Aphelion Ground Station

Aphelion Ground Station (GCS) is a browser-based telemetry dashboard designed to monitor **CanSat and Rocket** telemetry in real time.

The dashboard provides a shared map, telemetry data panels, altitude graphs, attitude visualization, telemetry logs, and CanSat live video support.

> **Current version:** Browser-based GCS  
> **Recommended browser:** Google Chrome or Microsoft Edge (desktop)

---

## ✨ Features

### 🛰️ CanSat

- GPS altitude
- Barometric altitude
- GPS satellite count
- Atmospheric pressure
- Temperature
- Roll / Pitch / Yaw
- Live altitude graph
- Live position on the map
- GPS trail
- Telemetry packet log
- Live video input from a USB capture device

### 🚀 Rocket

- GPS altitude
- Barometric altitude
- GPS satellite count
- Atmospheric pressure
- Temperature
- Roll / Pitch / Yaw
- Live altitude graph
- Live position on the shared map
- GPS trail
- Telemetry packet log

### 🗺️ Shared Live Map

Both the CanSat and Rocket are displayed on the same Leaflet map.

- 🛰️ CanSat marker
- 🚀 Rocket marker
- Position trail
- Automatic map centering when the first valid GPS position is received

---

## 📡 Telemetry Format

The current GCS expects telemetry lines beginning with:

```text
$TLM,
```

The parser expects the following fields:

```text
$TLM,ms,lat,lon,gpsAlt,sats,pressHpa,baroAlt,baroTemp,roll,pitch,yaw,...
```

The dashboard uses:

| Field | Description |
|---|---|
| `ms` | Time in milliseconds |
| `lat` | GPS latitude |
| `lon` | GPS longitude |
| `gpsAlt` | GPS altitude |
| `sats` | Number of GPS satellites |
| `pressHpa` | Pressure in hPa |
| `baroAlt` | Barometric altitude |
| `baroTemp` | Barometric temperature |
| `roll` | Roll angle |
| `pitch` | Pitch angle |
| `yaw` | Yaw angle |

Lines that do not start with `$TLM,` are ignored.

---

# 🖥️ Running the GCS Locally

## Option 1 — Open directly

Download or clone the repository and open:

```text
index.html
```

However, some browser features such as Web Serial and camera access work more reliably when the page is served from a local web server.

## Option 2 — Use a local web server

If you have Python installed:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

---

# 🔌 Connecting CanSat / Rocket Telemetry

1. Connect the telemetry device to your computer using USB.
2. Open the Aphelion GCS website in **Chrome or Microsoft Edge on desktop**.
3. Select the correct baud rate.
4. Click **Connect Serial**.
5. Select the appropriate serial device when prompted.
6. Start sending telemetry.
7. The dashboard should begin updating when valid `$TLM` packets are received.

### Default baud rate

The default baud rate is:

```text
9600
```

Available options:

```text
9600
19200
57600
115200
```

The baud rate must match the telemetry device.

---

# 📷 CanSat Live Video

The CanSat panel supports video input through a camera or USB video capture device.

Typical setup:

```text
VTX
 ↓
VRX
 ↓
USB Capture Card
 ↓
Computer
 ↓
Aphelion GCS
```

To enable video:

1. Connect the camera/capture device.
2. Open Aphelion GCS.
3. Click **Enable camera list**.
4. Allow camera permission.
5. Select the desired video device.
6. The video should appear in the CanSat panel.

---

# 🌐 Deploying with GitHub Pages

Aphelion GCS can be hosted as a static website using GitHub Pages.

Your repository should contain:

```text
AphelionGCS/
├── index.html
├── A.png
├── ITB_Logo.png
└── README.md
```

### Steps

1. Create a GitHub repository.
2. Upload the GCS files.
3. Make sure `index.html` is in the repository root.
4. Go to:

```text
Settings → Pages
```

5. Under **Build and deployment**, select:

```text
Source: Deploy from a branch
Branch: main
Folder: / (root)
```

6. Click **Save**.
7. GitHub will provide your public Pages URL.

It will look similar to:

```text
https://YOUR-USERNAME.github.io/AphelionGCS/
```

---

# ⚠️ Important: Current Internet Architecture

The current version uses the browser's **Web Serial API**.

That means the website itself does **not** automatically receive telemetry from a remote CanSat or Rocket over the Internet.

Current architecture:

```text
CanSat / Rocket
      │
      ▼
USB / Serial
      │
      ▼
Computer
      │
      ▼
Chrome / Edge
      │
      ▼
Aphelion GCS
```

Publishing the website on GitHub Pages makes the dashboard publicly accessible, but each computer still needs its own compatible serial connection.

For a future online system where everyone sees the **same live telemetry**, the architecture should be expanded to:

```text
CanSat ──────┐
             │
Rocket ──────┤
             ▼
       Ground Station
             │
             ▼
      Telemetry Server
             │
       ┌─────┼─────┐
       ▼     ▼     ▼
     User 1 User 2 User 3
```

This would allow multiple viewers to watch the same live mission telemetry from phones, laptops, or other devices.

---

# 🛠️ Project Structure

```text
AphelionGCS/
│
├── index.html          # Main GCS application
├── A.png               # Aphelion logo
├── ITB_Logo.png        # ITB logo
└── README.md           # Project documentation
```

The current HTML application loads:

- **Chart.js** for telemetry graphs
- **Leaflet** for the map
- **OpenStreetMap** map tiles
- Browser **Web Serial API** for serial telemetry
- Browser **MediaDevices API** for video input

---

# 🔧 Troubleshooting

### "Web Serial not supported"

Use:

- Google Chrome desktop, or
- Microsoft Edge desktop

The current implementation is not intended for mobile browsers.

### Telemetry is connected but values don't update

Check:

1. Baud rate
2. Serial device
3. Telemetry packet format
4. Packet begins with `$TLM,`
5. Packet contains the expected fields
6. Telemetry lines end with a newline

Example:

```text
$TLM,1000,-6.9000,107.6000,120.5,12,1008.2,118.7,27.4,1.2,3.4,45.6,...
```

### Map doesn't appear

The map requires an Internet connection because the current implementation loads Leaflet and OpenStreetMap resources from external URLs.

---

# 📜 License

```text
Copyright © Aphelion Team / ITB
```

---

# 🚀 Aphelion Ground Station

**CanSat & Rocket Telemetry Monitoring System**

Built for telemetry visualization, mission monitoring, and ground-station operations.
