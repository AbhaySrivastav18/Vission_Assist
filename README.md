<<<<<<< HEAD
# AI Vision Assist

Real-time obstacle detection and voice guidance system for visually impaired users.

---

## Project Structure

```
vis-assist/
├── models/
│   └── yolov8n.pt          ← download separately (see below)
│   └── yolov8x.pt          ← download separately (see below)
├── src/
│   ├── __init__.py
│   ├── voice.py            ← thread-safe TTS with priority queue
│   ├── detection.py        ← YOLOv8 wrapper
│   ├── depth.py            ← MiDaS DPT_Hybrid wrapper
│   └── navigation.py       ← position/distance analysis & message generation
├── obstacle_detection.py           ← basic mode (no depth, CPU-friendly)
├── obstacle_detection_upgraded.py  ← full AI pipeline (depth + GPU)
├── requirements.txt
└── README.md
```

---

## Quick Start

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

For GPU support (recommended):
```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118
```

### 2. Download YOLO weights

```bash
# Nano model (basic mode)
python -c "from ultralytics import YOLO; YOLO('yolov8n.pt')"
mv yolov8n.pt models/

# Large model (upgraded mode — more accurate)
python -c "from ultralytics import YOLO; YOLO('yolov8x.pt')"
mv yolov8x.pt models/
```

MiDaS is loaded automatically via `torch.hub` on first run.

### 3. Run

**GUI mode** (recommended — full dashboard):
```bash
python gui.py
```

**Basic CLI mode** (lightweight, CPU-only):
```bash
python obstacle_detection.py
```

**Upgraded CLI mode** (depth-aware, GPU-accelerated):
```bash
python obstacle_detection_upgraded.py
```

---

## GUI Dashboard (`gui.py`)

```
┌─────────────────────────────────────────────────────────────┐
│  ◈ AI VISION ASSIST   [Mode ▾]  [▶ START]                   │
├──────────────────────────────┬──────────────────────────────┤
│                              │  DETECTIONS                  │
│     LIVE CAMERA FEED         │  person  95%  ████░░  d=0.28 │
│   (bounding-box overlay)     │  car     81%  ██░░░░  d=0.45 │
│                              │  SETTINGS                    │
│   🔴 CRITICAL — STOP         │  Confidence ━━●━━  60%       │
│  "person very close. Turn."  │  Alert delay ━●━━  1.5s      │
│                              │  ☑ Depth heatmap overlay     │
│                              │  ☑ Voice alerts enabled      │
│                              │  ALERT LOG                   │
│                              │  [12:04:01] CRITICAL …       │
├──────────────────────────────┴──────────────────────────────┤
│  FPS: 18.3  │  Depth: ON ✓  │  Device: GPU  │  Mode: Upg.  │
└─────────────────────────────────────────────────────────────┘
```

### GUI Features

| Feature | Description |
|---------|-------------|
| Live feed | Camera stream with colour-coded bounding boxes |
| Mode selector | Switch Basic / Upgraded without restarting |
| Alert badge | Red / Orange / Blue pill shows current urgency |
| Detection rows | Per-object proximity bar (green → orange → red) |
| Confidence slider | Adjust YOLO threshold in real time |
| Alert delay slider | Control voice cooldown (0.5 s – 5 s) |
| Depth overlay | Toggle MiDaS heat-map over the video |
| Voice toggle | Enable / disable TTS without stopping the pipeline |
| Alert log | Colour-coded, time-stamped history of all alerts |
| Status bar | Live FPS, device, depth status, mode |

---

| Key | Action |
|-----|--------|
| `Q` | Quit |
| `D` | Toggle depth heat-map overlay |
| `H` | Toggle on-screen help |

---

## How It Works

```
Camera Input
    │
    ▼
YOLOv8 Object Detection  ──────────────────────────────────────────┐
    │                                                               │
    ▼                                                               │
MiDaS Depth Estimation (every 3 frames)                            │
    │                                                               │
    ▼                                                               │
Navigation Analysis                                                 │
  • Horizontal zone: LEFT / CENTER / RIGHT                          │
  • Depth zone: very close / close / medium                         │
  • Urgency: critical / warning / info                              │
    │                                                               │
    ▼                                                               ▼
Priority Voice Alert               Annotated Video Window
"person very close. Turn left."    (bounding boxes + depth overlay)
```

---

## Depth Colour Coding

| Colour | Meaning |
|--------|---------|
| 🔴 Red    | Very close — immediate action required |
| 🟠 Orange | Close — caution |
| 🟢 Green  | Far — safe |

---

## Configuration

Edit the `Configuration` block at the top of either main script:

| Variable       | Default | Description |
|----------------|---------|-------------|
| `CONFIDENCE`   | 0.60    | Minimum YOLO detection confidence |
| `ALERT_DELAY`  | 1.2 s   | Cooldown between voice alerts |
| `FRAME_SKIP`   | 3       | Depth inference every N frames |
| `SHOW_WINDOW`  | True    | Show OpenCV preview window |

---

## Future Improvements

- Convert depth values to real-world metres via calibration
- Wearable smart-glasses integration (Raspberry Pi / Jetson)
- Mobile application interface
- Spatial / binaural audio guidance
- Path planning and route suggestion
=======
# Vission_Assist
An AI-powered assistive system for visually impaired users that uses YOLOv8 for real-time object detection and MiDaS for depth estimation. Obstacles are classified into Critical, Warning, and Info levels with priority-based TTS voice alerts. Built with FastAPI, WebSocket, PyQt5, and React for a live dashboard with depth heatmap overlay.
>>>>>>> 28ad61a7bf0ec9c8380c5f1aca341abb8411f516
