# Allsky YOLO Raindrop Detector

This repository documents a dissertation project focused on **localised rainfall detection** using an Allsky camera installed at **Queen Elizabeth Olympic Park**. The project integrates a custom-trained **YOLOv8** model to identify raindrops on the camera lens, record the **start of rainfall**, and communicate the result through the Allsky web overlay.

In addition to the detection module, this repo includes the physical **3D-printed camera enclosure**, **system deployment photos**, and a **fully reproducible module** installable in the Allsky platform.

---

## 📸 Project Context

- **Location**: One Pool Street rooftop, Queen Elizabeth Olympic Park, London
- **Camera**: Upward-facing fisheye lens Allsky camera
- **Goal**: Detect the **first raindrop** hitting the lens to signal onset of local rainfall
- **System**: Runs 24/7 on Raspberry Pi 4B, integrated with Allsky software suite

---

## 🧠 YOLOv8 Raindrop Detection Module

This module is triggered after every image capture in the Allsky system. It runs YOLOv8 object detection on the image, searching for lens raindrops. Once **2 detections within 3 minutes** occur, the module marks this as a rainfall event, records the first raindrop time, and enters a **cooldown period** to avoid redundant detections.

### ✅ Output Variables for Overlay

```json
{
  "AS_YOLORAINDETECTED": true,
  "AS_YOLOFIRSTDROP": "18 Aug 2025, 14:32"
}
```

These are automatically updated and displayed on the Allsky live web UI overlay.

---

## 🛠 Installation

This module is designed for Allsky version **v2023.05.01_04** or later.

### 1. Clone this repository
```bash
git clone https://github.com/yourusername/allsky-raindetector.git
```

### 2. Copy the module to Allsky
```bash
cp allsky-raindetector/module/yolo_dis_one.py /opt/allsky/modules/
```

### 3. Set up Python environment
```bash
cd /opt/allsky/modules/
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

> Ensure `ultralytics`, `opencv-python`, and `allsky_shared` are installed in the venv.

### 4. Add the module in Allsky config

Edit your `config.json` to include this under `"modules"`:

```json
{
  "module": "raindetector",
  "enabled": true,
  "events": ["day", "night"],
  "model_path": "/home/hanmc/yolo/weights/best.pt"
}
```

### 5. Restart Allsky and check logs
```bash
sudo systemctl restart allsky
tail -f /var/log/allsky.log
```

Once enabled, check the overlay or log output to see if detection is working correctly.

---

## 📁 Repository Structure

```
📦 allsky-raindetector/
├── README.md                 ← This file
├── images/                   ← Photos of device and output samples
│   ├── camera_setup.jpg
│   ├── deployment_site.jpg
│   └── sample_detection.jpg
├── 3d-models/                ← 3D printable enclosure designs
│   └── enclosure_v1.stl
├── module/                   ← Allsky module files
│   ├── yolo_dis_one.py       ← Main detection script
│   ├── requirements.txt      ← Dependencies (e.g. ultralytics, opencv-python)
│   └── config_example.json   ← Example config snippet
```

---

## 👨‍🔧 Author & Contact

Developed by **hanmc** for UCL CASA0022 Dissertation Module.

- 📧 Email: [your.email@example.com]
- 🔗 GitHub: [https://github.com/yourusername]

---

## 📌 Notes

- This module assumes a trained YOLOv8 model (`.pt`) already exists.
- Model should be trained on your own annotated lens raindrop dataset.
- Recommended image size during training: 1440×1080 (to match Allsky output).

---

## 📷 Image Samples

> Available under `images/` folder. Includes system setup and detection result visuals.

---

## 📦 3D Enclosure

> The 3D-printable STL files are included in `3d-models/`. Designed with Fusion 360, printed in **PLA**. Includes semi-open dome for lens exposure to raindrops.

---

## 📄 License

MIT License
