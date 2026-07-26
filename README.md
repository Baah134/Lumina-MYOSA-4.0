# Lumina: Active Guardian Smart Home Sensor

> *"Lumina doesn't just measure the room; it understands how the environment affects the human inside it."*

---

## 📖 Overview

Current smart home devices are "passive"—they show you numbers but don't explain what they mean. **Lumina** is an Active Guardian. It combines a **Wearable Node** (Activity Tracking) with a **Stationary Hub** (Environmental Sensing) to monitor both the user and their surroundings.

Lumina operates on a **Hybrid Edge-Cloud Architecture**. It uses a local server to aggregate and visualize sensor data in real-time. For complex analysis, it employs a gateway that sends raw telemetry to the **Nvidia Nemotron LLM** (via OpenRouter). This allows the system to offer advanced, context-aware intelligence directly to the user based on environmental and physical states.

### Key Features
* **AI-Driven Context:** Converts raw sensor data (e.g., `"1003 mbar"`, `"6 Lux"`) into human-readable advice (e.g., *"Storm approaching, secure the windows"*).
* **Fall & Activity Detection:** Uses accelerometer data to detect falls or sleep restlessness based on impact and body orientation.
* **Holistic Monitoring:** Simultaneously tracks Light, Pressure, Temperature, and Motion to build a complete picture of the user's environment.

---

## 🖼️ Demo / Examples

<table>
  <tr>
    <td align="center" width="50%">
      <img src="media/lumina-cover.png" alt="Figure 1: The Lumina Device and Dashboard Setup" width="100%"/>
      <br />
      <sub><b>Figure 1:</b> The Lumina Device and Dashboard Setup</sub>
    </td>
    <td align="center" width="50%">
      <img src="media/lumina-dashboard.jpg" alt="Figure 2: Real-time Data Visualization and Hybrid AI Chat Interface" width="100%"/>
      <br />
      <sub><b>Figure 2:</b> Real-time Data Visualization and Hybrid AI Chat Interface</sub>
    </td>
  </tr>
</table>

---

## ✨ Features (Detailed)

### 1. Environmental Telemetry
The stationary hub continuously monitors the "health" of the room:
* **Barometric Pressure:** Predicts incoming storms and weather changes (e.g., sudden drops < 1000 mbar).
* **Ambient Light:** Detects if the lighting is sufficient for the user's current activity (Reading vs. Sleeping) based on the time of day.
* **Temperature:** Monitors for heat stress risks using accurate environmental sensors.

### 2. Activity & Safety Monitoring
The accelerometer module (simulating a wearable tag) tracks the user's physical state:
* **Fall Detection:** Identifies sudden high-G impacts followed by a change in tilt orientation (user becoming horizontal).
* **Sleep/Rest Analysis:** Tracks micro-movements to determine if a user is restless or sleeping soundly.

### 3. AI Analysis
* **Cloud Intelligence:** Complex queries combining time, sensor states, and historical context (e.g., *"Is this combination of pressure drop and restlessness dangerous?"*) are routed to the Nvidia Nemotron LLM.
* **Contextual Memory:** The system retains a sliding window of previous sensor states, allowing the AI to compare current readings against recent history for better accuracy.

---

## 🚀 Installation & Setup Guide

Since Lumina uses a hybrid local-cloud architecture, you need to set up the local server before powering on the device.

### 1. Database Configuration
The system requires a local MySQL database to store sensor history.
1. Install **XAMPP** (or any WAMP stack).
2. Start **Apache** and **MySQL** from the XAMPP Control Panel.
3. Open your browser and go to `http://localhost/phpmyadmin`.
4. Create a new database named `sensor_db`.
5. Run the following SQL command to create the required table:

```sql
CREATE TABLE readings (
    id INT(6) UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    reading TEXT NOT NULL,
    reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

### 2. Backend Deployment
1. Navigate to your XAMPP installation folder (usually `C:\xampp\htdocs\`).
2. Create a new folder named `myosa_project`.
3. Copy the PHP files from the `backend/` folder of this repository (`test_data.php`, `dashboard.php`, `export_csv.php`) into that directory.

> ⚠️ **Note:** Ensure your `test_data.php` has your valid OpenRouter API Key. *(Never commit your API key to GitHub!)*

### 3. Hardware Assembly & Firmware
* **Wiring:** Connect sensors (BMP280, MPU6050, ADPS9960) via I2C (`SDA` -> GPIO 21, `SCL` -> GPIO 22).
* **IP Config:** Open the code in `firmware/esp_code.ino` and update the `String URL` with your laptop's local IP address (e.g., `192.168.1.5`).
* **Upload:** Flash the code to the ESP32 using the Arduino IDE.

---

## 🎮 Usage Instructions

To run the system once installed:
1. **Start the Local Server:** Ensure XAMPP is running Apache and MySQL.
2. **Power the ESP32:** Connect the ESP32 to a power source. It will automatically connect to WiFi and begin broadcasting sensor packets every 5 seconds.
3. **View the Dashboard:** Open a browser and navigate to:
   ```text
   http://localhost/myosa_project/dashboard.php
   ```

---

## 🛠️ Tech Stack & Requirements

### Tech Stack
* **Hardware:** ESP32 Microcontroller, Accelerometer (MPU6050), Pressure/Temp (BMP280), Light (BH1750), OLED Display.
* **Firmware:** C++ (Arduino Framework).
* **Backend:** PHP, MySQL (XAMPP Local Server).
* **AI Model:** Nvidia Nemotron-3-Nano-30b (via OpenRouter API).
* **Frontend:** HTML/CSS, Chart.js for real-time graphing.

### Hardware Dependencies
* ESP32 Dev Module
* Sensors (MPU6050, BMP280, ADPS9960)

### Software Dependencies
```bash
# For the AI Server interaction (if running Python backend)
pip install openai requests

# Local Server Setup
# Download and install XAMPP (Apache + MySQL)
```

---

## 🤝 Contribution Notes

We are actively looking for contributors to help expand Lumina's capabilities. We welcome pull requests and feature suggestions!

### Key Roadmap Items:
* **Voice Feedback Loop:** Integration of an I2S Speaker (MAX98357A) to allow Lumina to verbally announce alerts (Text-to-Speech) for visually impaired users.
* **Voice Command Interface:** Adding an INMP441 Microphone so users can query the AI naturally without a dashboard.
* **TinyML Integration:** Replacing the current threshold-based fall detection with a trained TensorFlow Lite model (via Edge Impulse) for higher accuracy.
* **Smart Home Bridging:** Adding MQTT support to allow Lumina to directly control smart bulbs and thermostats based on its environmental analysis.
