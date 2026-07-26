# Lumina: Active Guardian Smart Home Sensor

> *"Lumina doesn't just measure the room; it understands how the environment affects the human inside it."*

---

## 📖 Project Overview

Current smart home devices are "passive"—they display numbers but fail to explain what those metrics mean for the occupant. **Lumina** is an Active Guardian system that combines a **Wearable Node** (Activity Tracking) with a **Stationary Hub** (Environmental Sensing) to continuously monitor both the user and their immediate environment.

Lumina operates on a **Hybrid Edge-Cloud Architecture**. A local server aggregates and visualizes sensor data in real time. For advanced contextual analysis, a gateway routes telemetry to the **Nvidia Nemotron LLM** (via OpenRouter), converting physical and environmental data into actionable, human-readable insights.

---

## 🖼️ Media & Demos

All media assets referenced below are stored locally in the [`media/`](./media/) directory.

### Screenshots
| Figure 1 | Figure 2 |
| :---: | :---: |
| ![Figure 1: The Lumina Device and Dashboard Setup](media/figure1_device_dashboard.png) | ![Figure 2: Real-time Data Visualization and Hybrid AI Chat Interface](media/figure2_data_vis_ai.png) |
| *The Lumina Device and Dashboard Setup* | *Real-time Data Visualization and Hybrid AI Chat Interface* |

### Video Demonstration
* **[Watch Video 1: Live Demonstration of Sensor Data Triggering AI Responses](media/demo_video.mp4)**

> 💡 **Note:** To maintain a light repository size, it is recommended to host long demo videos on YouTube or Vimeo and link them directly here instead of tracking large `.mp4` files directly in Git.

---

## 🗺️ Project Roadmap

| Phase / Feature | Description | Status |
| :--- | :--- | :--- |
| **Environmental Telemetry** | Real-time tracking of barometric pressure, light, and temperature. | ✅ Complete |
| **Activity & Fall Detection** | Impact detection and posture analysis via wearable accelerometer. | ✅ Complete |
| **Hybrid Edge-Cloud AI** | Contextual telemetry analysis via Nvidia Nemotron LLM (OpenRouter). | ✅ Complete |
| **Voice Feedback Loop** | Integration of MAX98357A I2S speaker for spoken alerts (TTS). | 🔄 Planned |
| **Voice Command Interface** | INMP441 Microphone integration for direct voice queries to AI. | 🔄 Planned |
| **TinyML Edge Fall Model** | Replace threshold logic with a TensorFlow Lite model via Edge Impulse. | 🔄 Planned |
| **Smart Home Bridging** | Add MQTT support for direct control of smart bulbs and thermostats. | 🔄 Planned |

---

## ✨ Detailed Features

### 1. Environmental Telemetry
The stationary hub continuously monitors environmental health:
* **Barometric Pressure:** Detects rapid drops (< 1000 mbar) to predict incoming storms and pressure changes.
* **Ambient Light:** Evaluates lighting adequacy based on activity and time of day (e.g., reading vs. sleeping).
* **Temperature:** Monitors heat stress risks using precision environmental sensors.

### 2. Activity & Safety Monitoring
The accelerometer module (simulating a wearable tag) tracks physical state:
* **Fall Detection:** Identifies sharp high-G impacts paired with a sudden shift in orientation (user becoming horizontal).
* **Sleep / Rest Analysis:** Tracks subtle micro-movements to determine sleep quality vs. restlessness.

### 3. AI Contextual Intelligence
* **Cloud Intelligence:** Complex multi-variable queries (e.g., *"Is this pressure drop combined with user restlessness dangerous?"*) are routed to the Nvidia Nemotron LLM.
* **Sliding Window Memory:** Retains recent history so the AI can evaluate dynamic trends rather than isolated static readings.

---

## 📁 Repository Structure

```text
.
├── backend/
│   ├── dashboard.php        # Web interface for live charts and AI responses
│   ├── export_csv.php      # Export telemetry history
│   └── test_data.php       # Ingestion endpoint for ESP32 data
├── firmware/
│   └── esp_code.ino        # ESP32 C++ firmware (sensors + HTTP client)
├── media/
│   ├── figure1_device_dashboard.png
│   ├── figure2_data_vis_ai.png
│   └── demo_video.mp4      # Demo video asset
└── README.md
