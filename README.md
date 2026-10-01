# InnoVision

## AI-Driven Software-Defined Video Analytics Platform for Border Out Posts (BOPs)

InnoVision is an AI-driven tactical video analytics platform designed to transform conventional IP-based CCTV cameras into an intelligent surveillance and threat-detection network.

The platform combines computer vision, real-time video analytics, object tracking, facial recognition, automatic number plate recognition, behavioral anomaly detection, virtual perimeter monitoring, and a centralized command dashboard.

The system is designed to operate with existing camera infrastructure without requiring proprietary smart-camera hardware.

---

## 🚀 Key Features

### 1. Adaptive Low-Light Enhancement

InnoVision includes an adaptive low-light enhancement pipeline based on Zero-Reference Deep Curve Estimation (Zero-DCE).

The system evaluates scene brightness and can dynamically activate enhancement when the incoming video becomes too dark, improving visibility in low-light surveillance conditions.

### 2. Real-Time Object Detection & Tracking

The platform uses YOLO-based object detection together with ByteTrack for persistent object tracking.

This enables the system to maintain track IDs and track movement across the camera field of view.

### 3. Virtual Fence & Intrusion Detection

The platform supports configurable virtual security zones using polygon-based boundaries.

These zones can represent:

* Restricted perimeters
* Checkpost control areas
* Loitering zones
* International or sensitive boundaries

The system can detect boundary crossings and prolonged presence within defined areas.

### 4. Automatic Number Plate Recognition

The ANPR worker processes vehicle detections and performs:

* License plate localization
* Region-of-interest extraction
* Optical character recognition
* Plate matching
* Authorized/stolen vehicle registry comparison

### 5. Facial Recognition System

The FRS worker processes detected faces and generates facial embeddings for comparison against configured security watchlists.

The system uses ArcFace-based facial embeddings for identity matching.

### 6. Behavioral Anomaly Detection

InnoVision analyzes temporal movement patterns to identify unusual behavior.

The anomaly detection pipeline can be used to identify events such as:

* Rapid movement
* Perimeter climbing
* Unusual movement patterns
* Erratic activity

### 7. Asynchronous Processing Architecture

Heavy analytical tasks are separated from the primary video-ingestion pipeline using asynchronous workers and Redis messaging.

The architecture includes specialist workers for:

* ANPR
* Facial Recognition
* Behavioral Anomaly Detection

This separation helps maintain continuous video processing while secondary analytics run independently.

### 8. Central Command Dashboard

The project includes a React + Vite dashboard connected to a Flask-SocketIO backend.

The dashboard provides:

* Live video monitoring
* Real-time security alerts
* Alert snapshots
* Virtual fence configuration
* System telemetry
* GPU utilization information
* CPU utilization information
* Tracking information
* Night-vision enhancement status

---

## 🧠 System Architecture

```text
                    ┌───────────────────────┐
                    │   IP Camera / RTSP    │
                    │   USB Camera / Feed   │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │    Stream Engine      │
                    │  Video Ingestion      │
                    │  Low-Light Detection  │
                    │  Object Detection     │
                    │  Object Tracking     │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │    Redis Message      │
                    │       Broker          │
                    └───────────┬───────────┘
                                │
             ┌──────────────────┼──────────────────┐
             │                  │                  │
             ▼                  ▼                  ▼
      ┌────────────┐     ┌────────────┐     ┌────────────┐
      │    ANPR    │     │    FRS     │     │  Anomaly   │
      │   Worker   │     │   Worker   │     │   Worker   │
      └─────┬──────┘     └─────┬──────┘     └─────┬──────┘
            │                  │                  │
            └──────────────────┼──────────────────┘
                               ▼
                    ┌───────────────────────┐
                    │    Alert Manager     │
                    │  Alert Deduplication │
                    │  Rate Limiting        │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │  Flask-SocketIO API  │
                    └───────────┬───────────┘
                                │
                                ▼
                    ┌───────────────────────┐
                    │   React + Vite UI    │
                    │  Command Dashboard   │
                    └───────────────────────┘
```

---

## 📊 Project Components

| Component            | Purpose                                    |
| -------------------- | ------------------------------------------ |
| Stream Engine        | Video ingestion and real-time perception   |
| YOLO + ByteTrack     | Object detection and tracking              |
| Zero-DCE             | Low-light image enhancement                |
| Virtual Fence Engine | Perimeter and intrusion monitoring         |
| ANPR Worker          | License plate recognition                  |
| FRS Worker           | Facial recognition and watchlist matching  |
| Anomaly Worker       | Behavioral anomaly detection               |
| Redis                | Asynchronous message processing            |
| Flask-SocketIO       | Backend API and real-time communication    |
| React + Vite         | Command center dashboard                   |
| Shapely              | Polygon-based zone and boundary processing |

---

## 📁 Directory Structure

```text
InnoVision/
│
├── config/
│   ├── settings.py
│   └── watchlist.json
│
├── core/
│   ├── stream_engine.py
│   ├── virtual_fence.py
│   └── alert_manager.py
│
├── models/
│   ├── zero_dce.py
│   ├── face_engine.py
│   ├── plate_engine.py
│   └── anomaly_detector.py
│
├── workers/
│   ├── base_worker.py
│   ├── anpr_worker.py
│   ├── frs_worker.py
│   └── anomaly_worker.py
│
├── training/
│   ├── train_plate_detector.py
│   ├── train_yolo.py
│   └── prepare_dataset.py
│
├── dashboard/
│   └── app.py
│
├── dashboard-ui/
│   ├── src/
│   ├── package.json
│   └── vite.config.js
│
├── data/
├── logs/
├── tests/
│
├── InnoVision_Technical_Report.pdf
├── run_platform.py
├── setup.bat
├── requirements.txt
└── README.md
```

---

## ⚙️ Requirements

The project primarily uses:

* Python
* PyTorch
* OpenCV
* Ultralytics YOLO
* ByteTrack
* Redis
* Flask
* Flask-SocketIO
* React
* Vite
* Shapely
* PaddleOCR
* ArcFace
* CUDA-enabled GPU for accelerated processing

The project also includes configurations designed to work within limited GPU memory environments.

---

# 🛠️ Installation & Setup

## 1. Clone the Repository

```bash
git clone https://github.com/saxenaarya77/InnoVision.git
cd InnoVision
```

---

## 2. Automated Windows Setup

For Windows systems, the project provides an automated setup script.

Run:

```bash
setup.bat
```

---

## 3. Manual Python Environment Setup

Create a virtual environment:

```bash
python -m venv venv
```

Activate it on Windows:

```bash
venv\Scripts\activate
```

Install the required dependencies:

```bash
pip install -r requirements.txt
```

For CUDA-enabled systems, install the appropriate PyTorch version according to your CUDA configuration.

---

# ▶️ Running the Platform

## Synthetic Video Source

For testing without a physical camera:

```bash
python run_platform.py --source synthetic
```

## RTSP Camera

To use an IP camera:

```bash
python run_platform.py --source "rtsp://username:password@camera-ip:554/stream"
```

## USB Webcam

For a local webcam:

```bash
python run_platform.py --source 0
```

The backend server runs on:

```text
http://localhost:5000
```

---

# 🖥️ Running the Dashboard

Open another terminal and navigate to the frontend:

```bash
cd dashboard-ui
```

Install frontend dependencies:

```bash
npm install
```

Start the development server:

```bash
npm run dev
```

The frontend will normally be available at:

```text
http://localhost:5173
```

---

# 🎯 Dashboard Capabilities

The command dashboard provides several operational features.

### Virtual Fence Configuration

Users can draw polygon-based security zones directly on the video feed.

The zones can be configured for:

* Restricted perimeters
* Checkposts
* Loitering detection

### Tactical Incident Monitoring

The dashboard can display real-time events including:

* Intrusion alerts
* Watchlist matches
* Unauthorized vehicle plates
* Behavioral anomalies

### Hardware Monitoring

The dashboard can display system telemetry such as:

* GPU VRAM utilization
* CPU utilization
* Video FPS
* Active tracking information
* Low-light enhancement status

---

# 🧪 Model Training

The repository includes scripts for dataset preparation and model fine-tuning.

Prepare or verify the dataset:

```bash
python training/prepare_dataset.py
```

Run plate detector training:

```bash
python training/train_plate_detector.py
```

The training configuration is designed to operate within constrained GPU memory environments using techniques such as:

* Mixed precision
* Reduced batch sizes
* Controlled dataloader workers
* Dataset caching

---

# 📈 Performance

The original project documentation contains benchmark measurements for individual subsystems including perception, ANPR, facial recognition, behavioral anomaly detection, virtual fencing, and low-light enhancement.

Performance can vary depending on:

* Dataset
* Camera quality
* Lighting conditions
* Hardware
* Model configuration
* Detection thresholds

Therefore, benchmark values should be interpreted in the context of the project's testing environment.

---

# 👨‍💻 My Contribution

### Arya Krishna Saxena

**Team Member — InnoVision / Smart India Hackathon Project**

I contributed to the development of InnoVision as a member of the project team.

My involvement included contributing to the project's development process, collaborating with team members on implementation and refinement, and participating in testing and project-related technical work.

### Areas of Contribution

* Contributed to the development of the InnoVision project.
* Participated in technical discussions and project planning.
* Assisted with implementation and project refinement.
* Contributed to testing and debugging activities.
* Collaborated with team members during the development process.
* Worked on understanding and improving the project's technical components.

> **Note:** This repository represents a collaborative team project developed as part of our Smart India Hackathon work. Contributions were made collectively by the team.

---

# 🏆 Smart India Hackathon

InnoVision was developed as a team project for the **Smart India Hackathon (SIH)**.

The project focuses on applying artificial intelligence and computer vision to real-time surveillance and security monitoring.

---

# 📚 Documentation

A detailed technical report is included in the repository:

```text
InnoVision_Technical_Report.pdf
```

The report provides additional information about the project's architecture, implementation, and technical design.

---

# 🔐 Responsible Use

InnoVision is intended as a research and educational project for AI-based video analytics and security monitoring.

Features such as facial recognition, watchlists, vehicle identification, and behavioral monitoring involve sensitive data and should be deployed only with appropriate authorization, privacy safeguards, and compliance with applicable laws and organizational policies.

---

# 📌 Project Status

The project is under active development and may continue to evolve as new features, optimizations, and improvements are introduced.

---

## 👥 Team

**InnoVision — Smart India Hackathon Team**

Developed collaboratively by the project team at **VIT Bhopal University**.

---

## ⭐ Acknowledgements

We acknowledge the open-source libraries, frameworks, research, and developer communities that made the development of this project possible.

---

## 📄 License

No separate license has currently been specified for this repository.

If this project is later released as open-source, an appropriate license should be added.

