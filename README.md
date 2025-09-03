# 🚦 Intelligent Traffic Surveillance System (ITSS)

<p align="center">
  <img src="https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.12" />
  <img src="https://img.shields.io/badge/YOLO-v12-FF6F00?style=for-the-badge&logo=ultralytics&logoColor=white" alt="YOLOv12" />
  <img src="https://img.shields.io/badge/PyTorch-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white" alt="PyTorch" />
  <img src="https://img.shields.io/badge/OpenCV-5C3EE8?style=for-the-badge&logo=opencv&logoColor=white" alt="OpenCV" />
  <img src="https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=streamlit&logoColor=white" alt="Streamlit" />
  <img src="https://img.shields.io/badge/License-MIT-green?style=for-the-badge" alt="License" />
</p>

<p align="center">
  <strong>Automating Traffic Law Enforcement & Violation Detection using Attention-Centric Vision Models and Real-Time ANPR.</strong>
</p>

---

## 📌 Executive Overview

Urban vehicular congestion has expanded rapidly across metropolitan corridors, rendering manual roadside inspection inefficient, labor-intensive, and prone to human oversight.

The **Intelligent Traffic Surveillance System (ITSS)** is an end-to-end computer vision platform engineered for real-time automated traffic monitoring and compliance. Leveraging state-of-the-art **YOLOv12** attention-centric detectors, **ByteTrack** multi-object tracking, and vision-based OCR (Llama 4 Scout via Groq), the system continuously detects rule infractions, reads license plates, verifies vehicular document validity (PUC & Insurance), and generates digital e-challans with photographic evidence.

```
       [ CCTV / Video Stream ]
                  │
                  ▼
         [ YOLOv12 Detection ] ── (Vehicles, Signals, Riders, Plates)
                  │
                  ▼
         [ ByteTrack Tracking ] ── (Persistent Object ID & Trajectory)
                  │
       ┌──────────┴──────────────┬────────────────────────┐
       ▼                         ▼                        ▼
 🔴 Red Light Jump        ⚡ Speed Calculation       ⛑️ Helmet Violation
  (Stop-line Breach)     (Centroid Displacement)   (Rider & Pillion Check)
       │                         │                        │
       └─────────────────────────┼────────────────────────┘
                                 │
                                 ▼
                     [ ANPR: Plate Crop & OCR ]
                      (Regex Format Validation)
                                 │
                                 ▼
                   [ Compliance & RTO Registry ]
                    (Insurance & PUC Verification)
                                 │
                                 ▼
                 [ Automated E-Challan Generation ]
               (PDF Evidence • S3 Archive • SMS Alert)
```

---

## 🚀 Key Capabilities

### 1. 🚦 Visual Violation Detection
* **Red-Light Infraction Detection:** Tracks traffic signal cycles in real time and automatically flags vehicles crossing defined intersection stop-lines during red phases.
* **Helmet Non-Compliance:** Identifies motorcycle riders and isolates head regions to detect missing protective helmets on both rider and pillion passengers.
* **Kinematic Speed Estimation:** Computes vehicle displacement and centroid trajectories across video frames to gauge speed against prescribed speed thresholds.

### 2. 🔍 Automated Number Plate Recognition (ANPR)
* **High-Precision Plate Localization:** Employs high-resolution bounding box localization dedicated to vehicle registration plates under adverse weather and lighting conditions.
* **Vision OCR Processing:** Integrates Groq-accelerated multimodal vision models (`Llama-4-Scout-17B`) with regex-driven format validation for standard Indian registration plates (state-wise and Bharat Stage / BH series).

### 3. 📄 Automated Compliance & E-Challan Workflow
* **Document Status Verification:** Performs automated queries for active vehicle insurance and Pollution Under Control (PUC) certifications.
* **Digital Challan Dispatch:** Compiles violation timestamps, geographic location, infraction categories, vehicle particulars, and photographic proof into tamper-evident PDF citations.
* **Cloud Storage & Messaging:** Uploads violation records to secure cloud storage (AWS S3) and dispatches instant SMS alerts via Twilio.

---

## 🏗 System Architecture & Pipeline

| Pipeline Stage | Module / Engine | Description |
| :--- | :--- | :--- |
| **Object Detection** | **YOLOv12** (`yolo12x` / `yolo12m`) | Identifies vehicles, helmets, traffic signals, and registration plates with attention-enhanced feature extraction. |
| **Object Tracking** | **ByteTrack** | Preserves vehicle identities and continuous trajectories across frames, mitigating occlusions. |
| **OCR & Text Recognition** | **Llama-4-Scout-17B** (Groq Vision) | High-accuracy textual extraction from cropped plate regions. |
| **Document Compliance** | **RTO Verification Engine** | Validates real-time compliance status (Insurance / PUC expiration). |
| **Citation Generator** | **Jinja2 + pdfkit / wkhtmltopdf** | Dynamically synthesizes photographic e-challan reports. |
| **User Interface** | **Streamlit** | Interactive web dashboard for monitoring feeds, processing uploaded video, and reviewing infractions. |

---

## 📊 Benchmark & Model Performance

The evaluation metrics achieved across extensive benchmark validation on Indian traffic scenarios:

### Detection Architecture Comparison

| Model Architecture | mAP@50–95 | Accuracy | F1 Score |
| :--- | :---: | :---: | :---: |
| YOLOv11-l | 88.65% | 97.49% | 99.23% |
| YOLOv11-x | 87.81% | 98.24% | 99.25% |
| YOLOv12-m | 86.66% | 93.29% | 97.94% |
| YOLOv12-l | 88.66% | 98.13% | 99.25% |
| **YOLOv12-x (Primary)** | **90.61%** | **98.87%** | **99.34%** |

### End-to-End Pipeline Accuracy

* **Overall System Accuracy:** `93.76%`
* **Plate Localization (ANPR):** `98.00%`
* **Vehicle Classification:** `97.56%`
* **Helmet Non-Compliance Detection:** `93.26%`
* **Plate Character Recognition (OCR):** `92.70%`

---

## 💻 Tech Stack

* **Core Language:** Python 3.12
* **Deep Learning Framework:** PyTorch, Ultralytics YOLOv12
* **Computer Vision:** OpenCV (`cv2`), ByteTrack
* **Vision Language OCR:** Groq API (Llama-4-Scout)
* **Web Application:** Streamlit
* **Document Engine:** `pdfkit`, `wkhtmltopdf`, Jinja2
* **Cloud & Communications:** AWS S3, Twilio API, RapidAPI RTO Gateway

---

## 📂 Repository Structure

```
Traffic/
├── checkers/              # Verification modules (PUC, insurance status)
├── detectors/             # Core detection algorithms (helmet, speed, red-light)
├── models/                # YOLO model weights and specifications
├── pdfgen/                # E-challan generation templates and utilities
├── sms/                   # Notification dispatcher modules
├── static/                # Static assets and media
├── template/              # HTML invoice / challan templates
├── tests/                 # Unit test suite
├── utils/                 # Coordinate conversion, image utilities, tracking helpers
├── videos/                # Sample input feeds for testing
├── violations/            # Exported evidence frames and violation logs
├── main.py                # Command-line execution entry point
├── streamlit.py           # Interactive web application interface
├── requirements.txt       # Project dependencies
└── README.md              # Project documentation
```

---

## 🛠️ Quick Start & Installation

### Prerequisites
* **Python 3.12+**
* **CUDA-compatible GPU** (recommended for real-time video processing)
* **wkhtmltopdf** (required for PDF e-challan compilation)
  * **macOS:** `brew install --cask wkhtmltopdf`
  * **Ubuntu/Debian:** `sudo apt-get install -y wkhtmltopdf`
  * **Windows:** Download from [wkhtmltopdf.org](https://wkhtmltopdf.org/downloads.html)

### 1. Clone the Repository
```bash
git clone https://github.com/midhunsomu/Traffic.git
cd Traffic
```

### 2. Set Up a Virtual Environment
```bash
# Create environment
python -m venv .venv

# Activate environment
# On Windows:
.venv\Scripts\activate
# On Linux / macOS:
source .venv/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Configure Environment Variables
Copy `.env.example` to `.env` and supply your API credentials:
```bash
cp .env.example .env
```
Key configuration parameters:
```ini
GROQ_API_KEY="your-groq-api-key"
X_RAPIDAPI_KEY="your-rapidapi-key"
X_RAPIDAPI_HOST="rto-vehicle-information-verification.p.rapidapi.com"
AWS_ACCESS_KEY="your-aws-access-key"
AWS_SECRET_KEY="your-aws-secret-key"
AWS_BUCKET_NAME="your-bucket-name"
TWILIO_ACCOUNT_SID="your-twilio-sid"
TWILIO_AUTH_TOKEN="your-twilio-auth-token"
TWILIO_PHONE_NUMBER="+1234567890"
ENABLE_NOTIFICATIONS=false
```

### 5. Model Weights
Place required model weights inside the `models/` folder:
* `yolo12n.pt` - Base vehicle and signal detector
* `helmet.pt` - Helmet detection classifier
* `train3_best.pt` - License plate localization model

---

## 🖥️ Usage

### CLI Execution
Run the full surveillance pipeline on a target video file:
```bash
python main.py
```

### Streamlit Web Dashboard
Launch the interactive web portal to upload video feeds, inspect detections, and preview violations:
```bash
streamlit run streamlit.py
```

---

## 📄 License & Attribution

This project is licensed under the **MIT License**.

If you reference or build upon this surveillance implementation in your research, please cite:

```bibtex
@inproceedings{samant2025itss,
  title     = {Intelligent Traffic Surveillance: A Vision-based System for Detecting Traffic Rule Violations},
  author    = {Samant, Harsh and Bhosale, Shreevardhan and Doshi, Kush and Kale, Dashrath and Shah, Rishabh},
  booktitle = {2025 2nd International Conference on Integration of Computational Intelligent System (ICICIS)},
  address   = {Pune, India},
  year      = {2025},
  publisher = {IEEE},
  doi       = {10.1109/ICICIS65613.2025.11371065}
}
```
