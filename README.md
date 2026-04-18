# SmartClass AI 🎓
### Real-Time Attendance & Attention Monitoring System

> Automatically marks attendance using face recognition and monitors student attention levels — all from a live camera feed, accessible through any browser.

---

## Demo

| Feature | Preview |
|---|---|
| Live face detection + recognition | Camera feed with bounding boxes, names & attention % |
| Attention classification | Green = Attentive 🟢 · Red = Distracted 🔴 |
| Auto alerts | Fires when a student is distracted for 3+ consecutive frames |
| Attendance reports | Downloadable CSV with timestamps, confidence & attention scores |

---

## How It Works

```
Webcam Input
     ↓
MTCNN Face Detector  →  Crops face region from frame
     ↓
MobileNetV2          →  Identifies the student (72.46% accuracy, 67 people)
     ↓
Custom CNN           →  Classifies attention: Attentive / Distracted (98.62% accuracy)
     ↓
Streamlit Dashboard  →  Live annotated feed + metrics + alerts + reports
```

---

## Project Structure

```
smartclass-ai/
│
├── 1_Data_Prep.ipynb              # Download datasets, preprocess, save to Drive
├── 2_Face_Recognition.ipynb       # Train MobileNetV2 face recognition model
├── 3_Attention_Model.ipynb        # Train custom CNN attention classifier
├── SmartClass_Dashboard.ipynb     # Write & launch the Streamlit dashboard
│
├── README.md
└── .gitignore
```

> **Note:** Model files (`*.h5`), data arrays (`*.npy`), and the label encoder (`*.pkl`) are stored in Google Drive at `/MyDrive/FaceAttendance/` and are not tracked by Git.

---

## Models

### Face Recognition — MobileNetV2
| Detail | Value |
|---|---|
| Architecture | MobileNetV2 (frozen) + custom Dense head |
| Input Size | 224 × 224 × 3 |
| Classes | 67 students |
| Training Data | LFW dataset + 5 custom people (304 photos total) |
| Test Accuracy | **72.46%** |
| Parameters | 2,603,139 |
| Saved As | `face_recognition.h5` |

### Attention Classifier — Custom CNN
| Detail | Value |
|---|---|
| Architecture | 3× Conv2D + BN + MaxPool → Dense(256) → Sigmoid |
| Input Size | 64 × 64 × 3 |
| Classes | Binary (Attentive / Distracted) |
| Training Data | Yawn-Eye Dataset (2,900 samples) |
| Test Accuracy | **98.62%** |
| Parameters | 1,274,305 |
| Saved As | `attention_classifier.h5` |

---

## Datasets

| Dataset | Source | Used For |
|---|---|---|
| LFW (Labeled Faces in the Wild) | [Kaggle](https://www.kaggle.com/datasets/atulanandjha/lfwpeople) | Face recognition base |
| Yawn Eye Dataset | [Kaggle](https://www.kaggle.com/datasets/serenaraju/yawn-eye-dataset-new) | Attention classification |
| Custom photos | Captured manually | 5 additional students |

---

## ▶How to Run

### 1. Run the notebooks in order

| Step | Notebook | What it does |
|---|---|---|
| 1 | `1_Data_Prep.ipynb` | Downloads LFW + Yawn-Eye, merges custom photos, saves `.npy` arrays |
| 2 | `2_Face_Recognition.ipynb` | Trains MobileNetV2, saves `face_recognition.h5` |
| 3 | `3_Attention_Model.ipynb` | Trains CNN, saves `attention_classifier.h5` |
| 4 | `SmartClass_Dashboard.ipynb` | Writes `app.py`, launches Streamlit via ngrok |

### 2. Launch the dashboard
Run all 4 cells in `SmartClass_Dashboard.ipynb`. 

### 3. Inside the dashboard
1. Click **Load Models** in the sidebar
2. Enter a session name (e.g. `CS101 Period 2`)
3. Go to **Monitor** tab and start capturing frames
4. Use **Reports** tab to download the attendance CSV

---

## Dashboard Features

| Tab | Feature |
|---|---|
| **Monitor** | Live camera feed with annotated face boxes · per-person attention bars · real-time metrics |
| **Monitor** | Auto-alert when a student is distracted for 3 consecutive frames |
| **Register Face** | Capture new student photos and save to Drive without retraining |
| **Reports** | Attendance summary table + full raw event log · export as CSV or save to Drive |

### Sidebar Controls
| Control | Default | Description |
|---|---|---|
| Confidence Threshold | 0.60 | Minimum score to register a face as known |
| Attention Alert Threshold | 40% | Score below which student is flagged distracted |
| New Period | — | Clears stats but keeps models loaded |
| Full Reset | — | Clears everything including models |

---

## Tech Stack

| Category | Tool |
|---|---|
| Deep Learning | TensorFlow / Keras |
| Face Detection | MTCNN |
| Face Recognition | MobileNetV2 (transfer learning) |
| Attention Classification | Custom CNN |
| Dashboard | Streamlit |
| Image Processing | OpenCV, Pillow |
| Data | NumPy, Pandas |
| Infrastructure | Google Colab (GPU) + ngrok tunnel |
| Storage | Google Drive |

---

## Known Limitations

- Face recognition accuracy (72.46%) is limited by small per-person training data (30–114 photos). More photos = higher accuracy.
- Attention is based on eye/face state only — no gaze tracking or head pose estimation yet.
- Single-frame capture (no continuous video stream).
- Session data is lost when Colab runtime resets.
- ngrok URL changes every Colab session.

---

## Future Improvements

- [ ] Add head-pose estimation and gaze tracking for richer attention signals
- [ ] Implement continuous OpenCV video stream instead of single-frame capture
- [ ] Integrate Google Sheets / Firebase for persistent cross-session storage
- [ ] Deploy to Hugging Face Spaces or GCP for a permanent URL
- [ ] Retrain face model with 200+ photos per student for better accuracy

---

## License

This project is for academic purposes.
