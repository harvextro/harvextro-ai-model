# 🌶️ Harvextro — AI Model (Object Detection)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python)
![YOLOv3](https://img.shields.io/badge/Model-YOLOv3--Tiny-red)
![Platform](https://img.shields.io/badge/Platform-Kaggle-20BEFF?logo=kaggle)
![Status](https://img.shields.io/badge/Status-In%20Development-yellow)
![License](https://img.shields.io/badge/License-MIT-green)

> AI-powered Scotch Bonnet chilli pepper detection and classification system — part of the **Harvextro** automated sorting project.

---

## 📌 Overview

**Harvextro** is an automated conveyor belt grading system designed to sort Scotch Bonnet chilli peppers by type and ripeness using computer vision. This repository contains the **AI/ML component** — a real-time object detection model that identifies and classifies chilli peppers captured by a camera module on the conveyor belt.

The model is built on **YOLOv3-Tiny** (Darknet) and trained to detect 5 classes of Scotch Bonnet produce directly from a live video stream.

---

## 🎯 Detected Classes

| Class ID | Label | Description |
|----------|-------|-------------|
| 0 | `yellow_chilli` | Fully ripe yellow Scotch Bonnet |
| 1 | `green_chilli` | Unripe green Scotch Bonnet |
| 2 | `mix_chilli` | Partially ripe / mixed colour |
| 3 | `red_chilli` | Fully ripe red Scotch Bonnet |
| 4 | `leaves` | Plant leaves (non-chilli, rejected) |

---

## 🧠 How the AI Model Works

```
Camera Feed
    │
    ▼
┌─────────────────────────────┐
│   YOLOv3-Tiny (416×416)     │  ← Object Detection
│   Darknet Framework         │
└─────────────────────────────┘
    │
    ▼
Bounding Box + Class + Confidence
    │
    ▼
Classification Result
(yellow / green / mix / red / leaves)
    │
    ▼
Signal to Conveyor Belt System
```

The model processes each video frame through a neural network and outputs bounding boxes with class labels and confidence scores. These results are then passed to the hardware layer to trigger the sorting mechanism on the conveyor belt.

---

## 🗂️ Repository Structure

```
harvextro-ai-model/
│
├── train.py                    ← Full training pipeline (run on Kaggle)
├── detect.py                   ← Run detection on a video file
├── requirements.txt            ← Python dependencies
├── .gitignore
│
├── scripts/
│   └── convert_dataset.py      ← Convert Roboflow export → YOLO format
│
└── docs/
    └── dataset_structure.md    ← Dataset format documentation
```

---

## ⚙️ Model Configuration

| Parameter | Value |
|-----------|-------|
| Architecture | YOLOv3-Tiny |
| Input Resolution | 416 × 416 |
| Number of Classes | 5 |
| Output Filters | 30 `= (5 + 5) × 3` |
| Batch Size | 64 |
| Max Batches | 6000 |
| Learning Rate | 0.0001 |
| Training Platform | Kaggle (GPU T4 × 2) |
| Estimated Train Time | ~10–20 minutes |

---

## 📦 Dataset

The dataset consists of real images of Scotch Bonnet chilli peppers, originally labeled using **Roboflow** and exported in YoloKeras format.

| Split | Images |
|-------|--------|
| Train | 18 |
| Valid | 5 |
| Test | 2 |
| **Total** | **25** |

### Dataset Format (YOLO Flat)
```
yolo_dataset/
├── obj/
│   ├── image1.jpg
│   ├── image1.txt      ← one line per object: "classID cx cy w h"
│   └── ...
├── train.txt
├── valid.txt
├── obj.names
└── obj.data
```

Each label file supports **multiple objects per image** — a single image can contain multiple chilli types simultaneously.

---

## 🚀 Setup & Usage

### Prerequisites
```bash
pip install -r requirements.txt
```

### Step 1 — Convert the dataset
```bash
# Place Scotch_bonnet.zip in the root folder, then run:
python scripts/convert_dataset.py
```
This converts the Roboflow export into YOLO flat format.
Upload the generated `yolo_dataset/` folder to **Kaggle** as a dataset.

### Step 2 — Train the model (on Kaggle)
1. Create a new Kaggle notebook
2. Add your `yolo_dataset` as a dataset input
3. Enable **GPU T4 × 2** accelerator
4. Update the dataset path in `train.py`:
```python
DATASET_PATH = '/kaggle/input/YOUR-DATASET-NAME/yolo_dataset/obj'
DATASET_ROOT = '/kaggle/input/YOUR-DATASET-NAME/yolo_dataset'
```
5. Run:
```python
!python train.py
```

### Step 3 — Run detection on video
After downloading the trained weights from Kaggle output:
```bash
python detect.py
```
Update `VIDEO_PATH` and `WEIGHTS` in `detect.py` to point to your files.

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|-----------|
| Object Detection | YOLOv3-Tiny (Darknet) |
| Training Platform | Kaggle (GPU T4 × 2) |
| Video Processing | OpenCV |
| Dataset Labeling | Roboflow |
| Language | Python 3.8+ |
| Hardware (full system) | Raspberry Pi, Camera Module, Conveyor Belt |

---

## 🔗 Related Repositories

> This is the AI module of the Harvextro project. Other components:

- [`harvextro`](https://github.com/harvextro/harvextro) — Main project repository
- [`harvextro-ai-model`](https://github.com/harvextro/harvextro-ai-model) — *(this repo)* AI detection model

---

## 👥 Team

**Harvextro** — CS 135
Supuni Wijesinghe (Leader)
Dilni Wijesinghe
Manthisi Herath
Thinara Senarathna
Imasha Kumarasiri
Kavinima Dulsadi


