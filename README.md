# 🏭 Warehouse Multi-Object Tracking System

Real-time detection aur tracking system for warehouse surveillance using **YOLOv8 + OpenCV**.

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

- 🔍 **3 Object Classes** detect karta hai — Person, Forklift, Parcel
- 🆔 **Unique Tracking IDs** — har object ko ek ID milti hai jo frames mein consistent rehti hai
- 📊 **Live Analytics HUD** — screen pe real-time count dikhata hai
- ↕️ **Entry / Exit Counting** — virtual counting line cross hone par count karta hai
- 📝 **CSV Log Export** — har frame ka data CSV file mein save hota hai
- 🎥 **Video Save** — annotated output video save kar sakte ho

---

## 📁 Project Structure

```
warehouse_tracker/
├── main.py                  ← Entry point - yahi chalao
├── requirements.txt         ← Python dependencies
├── utils/
│   ├── __init__.py          ← Empty file (required)
│   ├── tracker.py           ← Object tracking (IoU based)
│   ├── analytics.py         ← Entry/Exit counting
│   └── visualizer.py        ← OpenCV drawing (boxes, labels, HUD)
└── output/                  ← CSV logs aur video yahan save hote hain
```

---

## 📦 Requirements

| Requirement | Version |
|---|---|
| Python | 3.9 or higher |
| ultralytics | >= 8.0.0 |
| opencv-python | >= 4.7.0 |
| numpy | >= 1.23.0 |
| pandas | >= 1.5.0 |
| matplotlib | >= 3.6.0 |

---

## ⚙️ Installation

**Step 1 — Virtual environment banao aur activate karo**

```bash
# Windows (PowerShell)
python -m venv venv
venv\Scripts\activate

# Mac / Linux
python -m venv venv
source venv/bin/activate
```

**Step 2 — Dependencies install karo**

```bash
pip install -r requirements.txt
```

**Step 3 — Verify karo sab install hua**

```bash
python -c "import ultralytics, cv2, numpy; print('All good!')"
```

---

## 🚀 Usage

**Webcam se chalao (default)**
```bash
python main.py --source 0
```

**Video file process karo**
```bash
python main.py --source warehouse.mp4
```

**Better accuracy model use karo**
```bash
python main.py --source 0 --model yolov8s.pt
```

**Video save karo**
```bash
python main.py --source warehouse.mp4 --save
```

**RTSP IP Camera**
```bash
python main.py --source rtsp://192.168.1.100:554/stream
```

### All Arguments

| Argument | Default | Description |
|---|---|---|
| `--source` | `0` | Webcam index, video file path, ya RTSP URL |
| `--model` | `yolov8n.pt` | YOLOv8 weights file |
| `--output` | `output` | Output folder for CSV aur video |
| `--save` | False | Annotated video save karo |

> **Note:** Pehli baar chalane par YOLOv8 weights automatically download hote hain (~6MB for nano)

**Quit karne ke liye:** `Q` ya `ESC` press karo

---

## 📤 Output


### CSV Log
`output/log_TIMESTAMP.csv` mein save hota hai:

| Column | Example | Description |
|---|---|---|
| frame | 142 | Frame number |
| time | 14:32:05 | Time |
| track_id | 3 | Unique object ID |
| class | Person | Detected class |
| x1, y1, x2, y2 | 120,80,240,300 | Bounding box pixels |
| confidence | 0.87 | Detection confidence (0-1) |
| event | ENTRY | Line cross event (ENTRY/EXIT/blank) |

### Terminal Summary
Program band hone par yeh dikhata hai:
```
=========== FINAL SUMMARY ===========
  Person     | Total:  12 | IN:  8 | OUT:  4
  Forklift   | Total:   3 | IN:  2 | OUT:  1
  Parcel     | Total:  27 | IN: 15 | OUT: 12
```

---

## 🎯 Detected Classes

YOLOv8 default COCO model use hota hai. Warehouse ke liye yeh proxy classes hain:

| Label | COCO Classes Used | Class IDs |
|---|---|---|
| Person | person | 0 |
| Forklift | truck | 7 |
| Parcel | handbag, suitcase, bottle, chair, laptop, cell phone, book | 24, 28, 39, 56, 63, 67, 73 |

> **Better Accuracy ke liye:** Custom YOLOv8 model train karo warehouse data pe.
> Free datasets: [roboflow.com/search?q=warehouse](https://roboflow.com/search?q=warehouse)

---

## 🔧 Configuration

`main.py` ke top mein yeh values change kar sakte ho:

```python
CONF_THRESHOLD = 0.30   # Detection confidence (kam karo = zyada detect, zyada false positives)
IOU_THRESHOLD  = 0.45   # NMS threshold
LINE_POSITION  = 0.5    # Counting line position (0.0=top, 1.0=bottom)
```

### Model Size vs Speed (CPU)

| Model | File | Speed | Accuracy |
|---|---|---|---|
| YOLOv8 Nano | `yolov8n.pt` | Fastest (~20 FPS) | Lower |
| YOLOv8 Small | `yolov8s.pt` | Fast (~12 FPS) | Good ✅ |
| YOLOv8 Medium | `yolov8m.pt` | Moderate (~6 FPS) | Better |

---

## 🛠️ Troubleshooting

| Problem | Solution |
|---|---|
| `ModuleNotFoundError: utils.tracker` | File ka naam `tracker.py` hona chahiye (`tracer.py` nahi) |
| `ModuleNotFoundError: utils.visualizer` | File ka naam `visualizer.py` hona chahiye (`viz.py` nahi) |
| Webcam nahi khul raha | `--source 1` ya `--source 2` try karo |
| Sirf Person detect ho raha hai | `WAREHOUSE_CLASSES` mein saare class IDs check karo |
| Bahut slow chal raha hai | `yolov8n.pt` use karo ya frame resize karo |
| `venv\Scripts\activate` error | PowerShell mein run karo as Administrator |

---

## 📄 License

This project is for educational and surveillance purposes.

---

*Built with YOLOv8 + OpenCV + Python*
