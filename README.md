<p align="center">
  <img src="https://img.shields.io/badge/⚡_EDGE_AI-Powered-blueviolet?style=for-the-badge" alt="Edge AI"/>
  <img src="https://img.shields.io/badge/Models-4_Neural_Networks-orange?style=for-the-badge&logo=tensorflow" alt="4 Models"/>
  <img src="https://img.shields.io/badge/Privacy-100%25_Client--Side-green?style=for-the-badge&logo=shield" alt="Privacy"/>
</p>

<h1 align="center">🛡️ Face · Object · Gaze<br/>AI Proctoring System</h1>

<p align="center">
  <strong>The "Super Brain" — A next-generation, zero-latency, browser-based proctoring engine<br/>that runs 4 neural networks simultaneously on the client side.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/React-19-61DAFB?style=flat-square&logo=react&logoColor=white" alt="React"/>
  <img src="https://img.shields.io/badge/Vite-7-646CFF?style=flat-square&logo=vite&logoColor=white" alt="Vite"/>
  <img src="https://img.shields.io/badge/TensorFlow.js-4.22-FF6F00?style=flat-square&logo=tensorflow&logoColor=white" alt="TFjs"/>
  <img src="https://img.shields.io/badge/MediaPipe-FaceMesh-4285F4?style=flat-square&logo=google&logoColor=white" alt="MediaPipe"/>
  <img src="https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white" alt="Tailwind"/>
  <img src="https://img.shields.io/badge/License-MIT-purple?style=flat-square" alt="MIT"/>
</p>

---

## 🎯 What Is This?

A sophisticated **React + Vite** application engineered to ensure the integrity of online examinations — entirely in the browser.

Unlike traditional proctoring tools that rely on backend video streaming (high latency, high cost, privacy concerns), this system leverages **Edge AI** to run **all processing on the user's device**. No video leaves the browser. No images are stored. The biometric embeddings exist only in RAM during the session.

---

## 🧠 The "Super Brain" — 4 Models, 1 Mission

Four neural networks load in parallel to create a comprehensive, real-time surveillance mesh:

```
┌─────────────────────────────────────────────────────────────┐
│                    🧠 SUPER BRAIN ENGINE                    │
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │  MobileNetV2 │  │   COCO-SSD   │  │    BlazeFace     │  │
│  │  Identity 🔒 │  │  Objects 📱  │  │  Head Count 👥   │  │
│  │  128-d embed │  │  Phone/Book  │  │  Multi-person    │  │
│  └──────────────┘  └──────────────┘  └──────────────────┘  │
│                                                             │
│  ┌──────────────────────────────────────────────────────┐   │
│  │              MediaPipe Face Mesh (468 pts)            │   │
│  │     👁️ Gaze Deviation    |    👄 Lip Movement        │   │
│  │     Iris ratio tracking  |    Talking detection      │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
```

| # | Model | Function | What It Detects | Technology |
|---|-------|----------|-----------------|------------|
| 1 | **MobileNetV2** | 🔒 Identity Lock | Extracts 128-dimensional facial embeddings. Verifies the person sitting is the actual candidate using **Cosine Similarity**. | `tf.loadGraphModel` — Custom trained |
| 2 | **COCO-SSD** | 📱 Object Scanner | Scans the room for unauthorized items: **cell phones**, **books**, and other forbidden objects in real-time. | `@tensorflow-models/coco-ssd` |
| 3 | **BlazeFace** | 👥 Head Counter | Detects if **multiple people** appear in the frame or if the user **leaves their seat** (no face detected). | `@mediapipe/face-detection` |
| 4 | **Face Mesh** | 👁️👄 Behavior Analyzer | Tracks **468 facial landmarks** to detect **gaze deviation** (looking away from screen) and **lip movement** (talking/whispering). | `@mediapipe/face-mesh` |

---

## ✨ Feature Breakdown

### 🔐 Phase 1 — Identity Calibration ("The Lock")

The system captures a high-quality baseline of the user's face and generates a unique **Tensor Vector** — essentially a biometric fingerprint stored in browser memory.

- Extracts a **128-dimensional embedding** from the webcam feed
- Locks this vector as the session's identity reference
- All future frames are compared against this baseline

### 🛡️ Phase 2 — The Omni-Proctor Loop

Once locked, the system enters a continuous surveillance loop running at **~60 FPS**:

| Check | How It Works | Severity |
|-------|-------------|----------|
| **Identity Verification** | Calculates Cosine Similarity between live feed and locked baseline every frame | 🔴 Critical |
| **Object Detection** | COCO-SSD scans for phones & books every 500ms | 🟡 Warning |
| **Face Counting** | BlazeFace detects multiple faces or absence | 🔴 Critical |
| **Gaze Tracking** | Iris-to-eye-corner ratio detects looking away | 🟡 Warning |
| **Lip Movement** | Upper-lower lip distance detects talking | 🟡 Warning |

#### 🛡️ Robustness Layer — No False Positives

```
Strike System:  30 consecutive mismatched frames (~1 second) required to trigger alert
Cooldown Timer: 4-second gap between same-type alerts prevents spam
Throttling:     Heavy detections (objects, faces, gaze) run every 500ms, not every frame
```

### 🚫 Phase 3 — Browser Lockdown & Auto-Termination

The system enforces strict browser security during the exam:

| Action | Response |
|--------|----------|
| **Tab Switch** (`Alt+Tab`, clicking another tab) | ❌ **Instant exam termination** |
| **Window Focus Loss** (clicking outside browser) | ❌ **Instant exam termination** |
| **Copy/Paste** (`Ctrl+C`, `Ctrl+V`) | 🚫 Blocked & logged |
| **DevTools** (`F12`) | 🚫 Blocked & logged |
| **Print Screen** | 🚫 Blocked & logged |
| **Right-Click** | 🚫 Disabled |

When tab switching or focus loss is detected, the exam is **immediately terminated** with a detailed incident report showing the violation type, timestamp, and unique incident ID.

### 📊 Phase 4 — Post-Exam Audit Dashboard

After submission, a comprehensive dashboard displays:

- **Total Violation Count** with severity breakdown (Critical vs Warning)
- **Detailed Audit Log** — timestamped table of every violation
- **Integrity Score** based on session behavior

---

## 🏗️ Architecture & Tech Stack

```
src/
├── App.jsx                          # Main application — UI + Surveillance Loop
├── main.jsx                         # React entry point
├── index.css                        # Tailwind CSS v4
├── hooks/
│   ├── useProctorBrain.js           # 🧠 Loads all 4 AI models in parallel
│   └── useLockdown.js               # 🔒 Tab-switch detection & browser lockdown
├── components/
│   └── proctor/
│       └── Calibration.jsx          # Identity calibration UI
└── assets/

public/
└── models/
    ├── model.json                   # MobileNetV2 Face Recognition model manifest
    ├── group1-shard1of3.bin         # Model weights (4.0 MB)
    ├── group1-shard2of3.bin         # Model weights (4.0 MB)
    └── group1-shard3of3.bin         # Model weights (1.1 MB)
```

| Layer | Technology |
|-------|-----------|
| **Frontend** | React 19, Vite 7 |
| **Styling** | Tailwind CSS v4 (with `@tailwindcss/vite` plugin) |
| **AI / ML** | TensorFlow.js 4.22, MediaPipe Face Detection & Face Mesh |
| **Object Detection** | COCO-SSD (MobileNet v2 backbone) |
| **Face Recognition** | Custom MobileNetV2 (128-d embeddings, ~9.2 MB) |
| **State Management** | React Hooks + `useRef` for performance-critical paths |
| **Math Engine** | Custom Cosine Similarity, Tensor operations |
| **Icons** | Lucide React |

---

## 📦 Installation & Setup

### Prerequisites

- **Node.js** ≥ 18
- **npm** ≥ 9
- A device with a **webcam**
- A modern browser (Chrome/Edge recommended for WebGL support)

### Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/SBanditaDas/Face-Object-Gaze-Proctoring-Model.git
cd Face-Object-Gaze-Proctoring-Model

# 2. Install dependencies (fetches TF.js & MediaPipe binaries)
npm install

# 3. Launch the development server
npm run dev
```

Open **http://localhost:5173** and the Super Brain will begin initializing! 🧠

> **Note:** The first load may take a few seconds as TF.js downloads and initializes the COCO-SSD, BlazeFace, and Face Mesh models from CDNs. The Face Recognition model (9.2 MB) is bundled locally in `public/models/`.

---

## 🎮 Usage Guide

### Step 1 → Allow Camera Access
The app requires webcam permissions. Grant access when prompted.

### Step 2 → Wait for AI Initialization
Watch for the button to change from *"Initializing AI..."* to *"Lock Identity & Start Exam"*. This means all 4 models are loaded and ready.

### Step 3 → Lock Your Identity
Center your face in the webcam view and click **"Lock Identity & Start Exam"**. This captures your biometric baseline.

### Step 4 → Take the Exam
The UI switches to exam mode with a live **Match Score** and **Audit Trail**:

| Try This | Expected Detection |
|----------|-------------------|
| Hold up a **phone** 📱 | `UNAUTHORIZED_OBJECT: cell phone` |
| Hold up a **book** 📕 | `UNAUTHORIZED_OBJECT: book` |
| **Look left/right** 👀 | `LOOKING_AWAY_FROM_SCREEN` |
| **Talk or whisper** 🗣️ | `TALKING_DETECTED` |
| Have **another person** appear 👥 | `MULTIPLE_FACES_DETECTED` |
| **Leave the frame** 🚶 | `NO_FACE_IN_FRAME` |
| **Switch tabs** ⚠️ | `EXAM TERMINATED` — instant |
| **Cover your face** 🙈 | `IDENTITY_MISMATCH` (after ~1 sec) |

### Step 5 → End Session
Click **"End Exam"** to view the **Post-Exam Audit** dashboard with severity breakdown and full violation log.

---

## ⚠️ Privacy & Security

<table>
  <tr>
    <td>✅</td>
    <td><strong>100% Client-Side Processing</strong> — All video analysis runs locally on the user's device</td>
  </tr>
  <tr>
    <td>✅</td>
    <td><strong>Zero Server Communication</strong> — No video feeds, images, or biometric data are ever transmitted</td>
  </tr>
  <tr>
    <td>✅</td>
    <td><strong>Session-Only Memory</strong> — Facial embeddings exist only in browser RAM and are destroyed on page close</td>
  </tr>
  <tr>
    <td>✅</td>
    <td><strong>No Database Storage</strong> — No facial images or vectors are persisted anywhere</td>
  </tr>
</table>

---

## 🔧 Configuration & Tuning

Key thresholds can be adjusted in `App.jsx`:

| Parameter | Current Value | Location | Description |
|-----------|--------------|----------|-------------|
| Identity Threshold | `0.75` | Line ~133 | Cosine similarity below this = mismatch |
| Strike Threshold | `30 frames` | Line ~140 | Consecutive mismatches needed to trigger alert |
| Mismatch Cooldown | `4000ms` | Line ~140 | Gap between identity mismatch alerts |
| Detection Interval | `500ms` | Line ~147 | How often object/face/gaze checks run |
| Lip Open Threshold | `5px` | Line ~180 | Vertical lip distance to detect talking |
| Talking Cooldown | `2000ms` | Line ~180 | Gap between talking alerts |
| Gaze Deviation | `0.30 / 0.70` | Line ~202 | Iris ratio bounds for "looking away" |
| Gaze Cooldown | `2000ms` | Line ~202 | Gap between gaze alerts |

---

## 🤝 Contributing

Contributions are welcome! Please fork this repository and submit a Pull Request.

```bash
# 1. Fork the Project
# 2. Create your Feature Branch
git checkout -b feature/AmazingFeature

# 3. Commit your Changes
git commit -m 'Add some AmazingFeature'

# 4. Push to the Branch
git push origin feature/AmazingFeature

# 5. Open a Pull Request
```

---

## 📄 License

Distributed under the **MIT License**. See `LICENSE` for more information.

---

<div align="center">
  <br/>
  <strong>Built with ❤️ by S Bandita Das</strong>
  <br/>
  <sub>Powered by React · TensorFlow.js · MediaPipe</sub>
  <br/><br/>
  <img src="https://img.shields.io/badge/Made_with-React_&_TensorFlow.js-blue?style=for-the-badge" alt="Made with"/>
</div>
