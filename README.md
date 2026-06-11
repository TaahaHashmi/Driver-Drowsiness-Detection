# Real-Time Driver Drowsiness Monitoring System
**Course:** Computer Vision  
**Students:**
| Name | CMS ID |
|---|---|
| Muhammad Taaha Hashmi | 405513 |
| Abdussalam Sarmad | 403375 |
| Ameer Hamza | 412230 |

---

## Abstract

A real-time, non-intrusive driver monitoring system that uses computer vision to detect drowsiness, fatigue, and distraction. Achieves **94% drowsiness detection accuracy** at **25–30 FPS** on standard CPU hardware — no GPU required.

---

## How It Works

The system runs a modular pipeline on each video frame:

```
Webcam Feed → Face Detection → Landmark Extraction (68 pts)
    → EAR (Eye Closure) + MAR (Yawning) + Head Pose (PnP)
        → Threshold Logic → Visual Alert
```

### Detection Methods

| Feature | Method | Threshold | Alert |
|---|---|---|---|
| Drowsiness | Eye Aspect Ratio (EAR) | EAR < 0.15 for > 0.5s | EYES CLOSED |
| Fatigue | Mouth Aspect Ratio (MAR) | MAR > 0.20 for > 0.5s | YAWNING |
| Distraction | Head Pose (solvePnP / Yaw angle) | \|Yaw\| > 25° for > 10 frames | DISTRACTED |

**EAR Formula:**

```
EAR = (||p2 - p6|| + ||p3 - p5||) / (2 * ||p1 - p4||)
```

**MAR Formula:**

```
MAR = ||p62 - p66|| / ||p48 - p54||
```

---

## Results

| Metric | Value |
|---|---|
| Drowsiness Accuracy | 94% |
| Fatigue (Yawning) Accuracy | 91% |
| Distraction Accuracy | 88% |
| Frame Rate | 25–30 FPS |
| Alert Latency | < 150 ms |

---

## Tech Stack

- **Language:** Python
- **Face Detection:** Dlib (HOG + Linear SVM)
- **Landmark Model:** Dlib 68-point shape predictor
- **Head Pose:** OpenCV `solvePnP` + Rodrigues formula
- **Hardware:** Standard CPU (no GPU needed)

---

## Setup & Installation

```bash
# Clone the repo
git clone https://github.com/TaahaHashmi/Driver-Drowsiness-Detection
cd Driver-Drowsiness-Detection

# Install dependencies
pip install opencv-python dlib imutils numpy

# Download Dlib's 68-point landmark model
# shape_predictor_68_face_landmarks.dat
# http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2

# Run the system
python main.py
```

---

## Facial Landmark Regions Used

| Region | Landmark Indices |
|---|---|
| Left Eye | 36 – 41 |
| Right Eye | 42 – 47 |
| Mouth | 48 – 68 |
| Nose Tip | 30 |
| Chin | 8 |

---

## Limitations

- Struggles in **low-light** conditions (RGB camera only)
- **Glasses/masks** may occlude landmarks and reduce accuracy
- Thresholds are **generic** — not personalized per driver

---

## Future Work

- **Night Vision** — integrate IR sensors
- **Edge Deployment** — port to Raspberry Pi / NVIDIA Jetson
- **Personalized Calibration** — learn driver-specific baselines
- **Haptic Feedback** — vibrating steering wheel alerts

---

## References

1. WHO, "Road Traffic Injuries," 2024.
2. T. Soukupova and J. Cech, "Real-Time Eye Blink Detection using Facial Landmarks," CVWW 2016.
3. V. Kazemi and J. Sullivan, "One millisecond face alignment with an ensemble of regression trees," CVPR 2014.
