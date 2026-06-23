# NeuroPlay BCI

Real-time EEG visualization and biofeedback interface for [Macrotellect BrainLink](https://macrotellect.com) headsets. Built as a companion module for [NeuroPlay](https://github.com/yourname/neuroplay), a cognitive training platform for therapists and children.

Connects directly to BrainLink hardware via the Web Serial API — no middleware, drivers, or third-party software required.

## Features

- **Direct device connection** via Web Serial API (Chrome/Edge)
- **ThinkGear protocol parser** — full state-machine implementation with checksum validation
- **Live signal quality** monitoring with color-coded status
- **Attention & Meditation gauges** — animated eSense values (0–100)
- **Raw EEG waveform** — scrolling canvas chart at 512 Hz with auto-scaling
- **Brainwave power bands** — Delta, Theta, Alpha (low/high), Beta (low/high), Gamma (low/mid)
- **Session timeline** — dual-line chart tracking Attention and Meditation over time
- **Blink detection** with visual indicator
- **Session recording** with CSV export
- **Configurable** baud rate, buffer size, and timeline window

## Screenshots

> _Coming soon_

## Tech Stack

- HTML / CSS / JavaScript (vanilla, no frameworks)
- [Web Serial API](https://developer.chrome.com/docs/capabilities/serial) for device communication
- Canvas API for real-time chart rendering
- [ThinkGear Serial Stream Protocol](http://developer.neurosky.com/docs/doku.php?id=thinkgear_communications_protocol) for data parsing

## Requirements

- **Browser**: Google Chrome or Microsoft Edge (Chromium-based)
- **Device**: Macrotellect BrainLink headset (Pro, Lite, or compatible)
- **OS**: Windows, macOS, or Linux with Bluetooth support

## Getting Started

### 1. Pair your BrainLink

Open your system's Bluetooth settings and pair the BrainLink headset. On Windows, this creates a virtual COM port automatically.

### 2. Serve the app locally

The Web Serial API requires a secure context (`https://` or `localhost`). Use any static file server:

```bash
# Using npx (no install required)
npx -y serve ./ -l 3000

# Or using Python
python -m http.server 3000

# Or using PHP
php -S localhost:3000
```

### 3. Connect

1. Open `http://localhost:3000` in Chrome or Edge
2. Click **Connect**
3. Select the BrainLink's COM port from the browser dialog
4. Data begins streaming immediately

## Configuration

Click the ⚙️ **Settings** button to adjust:

| Setting | Options | Default |
|---------|---------|---------|
| Baud Rate | 9600 / 57600 / 115200 | 57600 |
| EEG Buffer Size | 512 / 1024 / 2048 / 4096 samples | 1024 |
| Timeline Window | 60s / 120s / 300s / 600s | 120s |

> **Note:** Most BrainLink models use **57600 baud**. If you receive no data after connecting, try switching to **115200**.

## Data Export

1. Click **⏺️ Start Recording** during a session
2. Click **⏹️ Stop Recording** when finished
3. Click **💾 Export CSV** to download

The CSV includes timestamped rows with: attention, meditation, signal quality, and all eight EEG power band values.

## Protocol Reference

This application implements the [ThinkGear Serial Stream Protocol](http://developer.neurosky.com/docs/doku.php?id=thinkgear_communications_protocol). Packet structure:

```
[SYNC 0xAA] [SYNC 0xAA] [PLENGTH] [PAYLOAD ...] [CHECKSUM]
```

Supported data codes:

| Code | Type | Description |
|------|------|-------------|
| `0x02` | Poor Signal | Signal quality (0 = good, 200 = no contact) |
| `0x04` | Attention | eSense attention value (0–100) |
| `0x05` | Meditation | eSense meditation value (0–100) |
| `0x16` | Blink | Blink strength (0–255) |
| `0x80` | Raw Wave | 16-bit signed raw EEG sample (512 Hz) |
| `0x83` | EEG Power | Eight 24-bit power band values |

## Project Structure

```
neuroplay-bci/
├── index.html      # Dashboard layout
├── index.css       # Design system (dark theme)
├── app.js          # Serial connection, parser, rendering
└── README.md
```

## Browser Compatibility

| Browser | Supported |
|---------|-----------|
| Chrome 89+ | ✅ |
| Edge 89+ | ✅ |
| Opera 76+ | ✅ |
| Firefox | ❌ (no Web Serial API) |
| Safari | ❌ (no Web Serial API) |

## Related

- [NeuroPlay](https://github.com/yourname/neuroplay) — Cognitive training app for therapists and children
- [BrainLinkParser-Python](https://github.com/Macrotellect/BrainLinkParser-Python) — Official Python parser by Macrotellect
- [brainlink-lsl](https://github.com/arnevdk/brainlink-lsl) — Lab Streaming Layer bridge for BrainLink

## License

MIT

## Contributing

Contributions are welcome. Please open an issue to discuss proposed changes before submitting a pull request.
