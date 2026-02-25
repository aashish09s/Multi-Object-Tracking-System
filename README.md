# 🏭 Warehouse Multi-Object Tracking System

A real-time object detection and tracking system for warehouse surveillance using **YOLOv8 + OpenCV**.

---

## 📋 Table of Contents

- [Features](#features)
- [Project Structure](#project-structure)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage](#usage)
- [Output](#output)
- [Detected Classes](#detected-classes)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)

---

## ✅ Features

- 🔍 **3 Object Classes** — Detects Person, Forklift, and Parcel
- 🆔 **Unique Tracking IDs** — Each object gets a consistent ID across all frames
- 📊 **Live Analytics HUD** — Real-time counts displayed directly on the video
- ↕️ **Entry / Exit Counting** — Counts objects crossing a virtual line
- 📝 **CSV Log Export** — Every frame's tracking data saved to a CSV file
- 🎥 **Video Save** — Option to save the fully annotated output video

---

## 📁 Project Structure

```
warehouse_tracker/
├── main.py                  ← Entry point — run this file
├── requirements.txt         ← Python dependencies
├── utils/
│   ├── __init__.py          ← Empty file (required for Python package)
│   ├── tracker.py           ← Multi-object tracking using IoU matching
│   ├── analytics.py         ← Entry/Exit event counting and statistics
│   └── visualizer.py        ← OpenCV drawing (boxes, labels, HUD overlay)
└── output/                  ← CSV logs and saved videos go here
```

---

## 📦 Requirements

| Requirement | Minimum Version |
|---|---|
| Python | 3.9 or higher |
| ultralytics | >= 8.0.0 |
| opencv-python | >= 4.7.0 |
| numpy | >= 1.23.0 |
| pandas | >= 1.5.0 |
| matplotlib | >= 3.6.0 |

---

## ⚙️ Installation

**Step 1 — Create and activate a virtual environment**

```bash
# Windows (PowerShell)
python -m venv venv
venv\Scripts\activate

# Mac / Linux
python -m venv venv
source venv/bin/activate
```

**Step 2 — Install all dependencies**

```bash
pip install -r requirements.txt
```

> This may take 3–8 minutes on the first run as it downloads PyTorch and other packages.

**Step 3 — Verify the installation**

```bash
python -c "import ultralytics, cv2, numpy; print('All good!')"
```

---

## 🚀 Usage

**Run with webcam (default)**
```bash
python main.py --source 0
```

**Process a recorded video file**
```bash
python main.py --source warehouse.mp4
```

**Use a more accurate model**
```bash
python main.py --source 0 --model yolov8s.pt
```

**Save the annotated output video**
```bash
python main.py --source warehouse.mp4 --save
```

**Connect to an IP / RTSP camera**
```bash
python main.py --source rtsp://192.168.1.100:554/stream
```

### All Arguments

| Argument | Default | Description |
|---|---|---|
| `--source` | `0` | Webcam index, video file path, or RTSP URL |
| `--model` | `yolov8n.pt` | YOLOv8 weights file |
| `--output` | `output` | Folder where CSV logs and video are saved |
| `--save` | False | Save the annotated output video |

> **Note:** On the first run, YOLOv8 model weights are downloaded automatically (~6 MB for nano).

**To quit:** Press `Q` or `ESC`

---


- **Top-right panel** — Live analytics showing total count, entries, and exits per class
- **Bottom-left** — Live FPS counter and current frame number

### CSV Log File

Saved to `output/log_TIMESTAMP.csv` after every run:

| Column | Example | Description |
|---|---|---|
| frame | 142 | Sequential frame number |
| time | 14:32:05 | Wall clock time |
| track_id | 3 | Unique persistent object ID |
| class | Person | Detected object class |
| x1, y1, x2, y2 | 120, 80, 240, 300 | Bounding box coordinates (pixels) |
| confidence | 0.87 | Detection confidence score (0.0 – 1.0) |
| event | ENTRY | Line crossing event — ENTRY, EXIT, or blank |

### Terminal Summary

Printed when the program exits:

```
=========== FINAL SUMMARY ===========
  Person     | Total:  12 | IN:  8 | OUT:  4
  Forklift   | Total:   3 | IN:  2 | OUT:  1
  Parcel     | Total:  27 | IN: 15 | OUT: 12
```

---

## 🎯 Detected Classes

This system uses the standard YOLOv8 COCO model. Since COCO does not include dedicated Forklift or Parcel classes, the following proxy mappings are used:

| Warehouse Label | COCO Classes Used | Class IDs |
|---|---|---|
| Person | person | 0 |
| Forklift | truck | 7 |
| Parcel | handbag, suitcase, bottle, chair, laptop, cell phone, book | 24, 28, 39, 56, 63, 67, 73 |

> **For better accuracy:** Train a custom YOLOv8 model on warehouse-specific data.
> Free labeled datasets are available at [roboflow.com/search?q=warehouse](https://roboflow.com/search?q=warehouse)

---

## 🔧 Configuration

You can adjust these constants at the top of `main.py`:

```python
CONF_THRESHOLD = 0.30   # Detection confidence threshold (lower = more detections, more false positives)
IOU_THRESHOLD  = 0.45   # Non-maximum suppression threshold
LINE_POSITION  = 0.5    # Counting line position (0.0 = top, 1.0 = bottom of frame)
```

### Model Size vs Speed (CPU)

| Model File | Speed (CPU) | Accuracy | Best For |
|---|---|---|---|
| `yolov8n.pt` | ~20 FPS | Lower | Real-time webcam |
| `yolov8s.pt` | ~12 FPS | Good ✅ | Balanced use |
| `yolov8m.pt` | ~6 FPS | Better | Recorded video |
| `yolov8l.pt` | ~3 FPS | Best | Offline processing |

---

## 🛠️ Troubleshooting

| Problem | Solution |
|---|---|
| `ModuleNotFoundError: utils.tracker` | Make sure the file is named `tracker.py` (not `tracer.py`) |
| `ModuleNotFoundError: utils.visualizer` | Make sure the file is named `visualizer.py` (not `viz.py`) |
| Cannot open webcam | Try `--source 1` or `--source 2` |
| Only Person is being detected | Check that all class IDs are listed in `WAREHOUSE_CLASSES` |
| Very low FPS | Use `yolov8n.pt` or reduce input resolution |
| `venv\Scripts\activate` fails | Run PowerShell as Administrator |
| `mkdir utils output` fails on PowerShell | Run `mkdir utils` and `mkdir output` separately |

---

## 📄 License

This project is intended for educational and surveillance purposes.

---

*Built with YOLOv8 · OpenCV · Python*

