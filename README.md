# Autonomous RC Car Racing Platform 🚗⚡
**Capstone Final Project — June 2026**[span_5](start_span)[span_5](end_span)  
*Software & Communication Subsystem*[span_6](start_span)[span_6](end_span)  

---

## 📌 About The Project
An advanced real-time platform designed to drive and monitor an RC car over Wi-Fi[span_7](start_span)[span_7](end_span). A human operator controls the physical RC car in real-time while tracking live telemetry[span_8](start_span)[span_8](end_span). Mobile and web dashboards communicate with a Python backend, which securely forwards validated commands to the vehicle's ESP32 microcontroller and streams continuous sensor data[span_9](start_span)[span_9](end_span).

* **Institution:** Bahçeşehir University — Department of Software Engineering[span_10](start_span)[span_10](end_span)  
* **Team:** Andranaz İlasbaş, Alkan Deniz, Yusuf Enes Arak[span_11](start_span)[span_11](end_span)  
* **Advisor:** Serkan Şimşek[span_12](start_span)[span_12](end_span)  

---

## 🏗️ Three-Tier Real-Time Architecture
Clients never touch the hardware directly—they communicate exclusively through the backend[span_13](start_span)[span_13](end_span).

1. **Clients (Mobile & Web):** React Native (Expo) & React (Vite, Recharts)[span_14](start_span)[span_14](end_span)
2. **Backend:** Python FastAPI (Uvicorn) handling state, websocket streams, and command validation (`backend/main.py`)[span_15](start_span)[span_15](end_span)
3. **Hardware:** ESP32 vehicle interface managing Servo (steering), ESC (throttle), Sonar, LiDAR, and IMU sensors[span_16](start_span)[span_16](end_span)

---

## 🛠️ Technology Stack
* **Mobile App:** React Native, Expo Go (Single codebase, on-device testing)[span_17](start_span)[span_17](end_span)
* **Web Dashboard:** React, Vite, Recharts (Fast build, live monitoring UI)[span_18](start_span)[span_18](end_span)
* **Backend:** Python, FastAPI, Uvicorn, HTTPS (Async, native WebSocket, OpenAPI)[span_19](start_span)[span_19](end_span)
* **Hardware:** ESP32, Servo, ESC (Embedded actuation & sensors)[span_20](start_span)[span_20](end_span)
* **Database:** InfluxDB 2.x (Time-series race history)[span_21](start_span)[span_21](end_span)
* **Communications:** WebSocket & HTTP REST (Low-latency full-duplex transport)[span_22](start_span)[span_22](end_span)

---

## 🔄 Backend & Real-Time Data Path
* **Poll ESP32:** Backend requests GET `/status` every 500 ms for fresh sensor snapshots[span_23](start_span)[span_23](end_span).
* **Build Telemetry:** `generate_telemetry()` maps real readings into a single JSON frame[span_24](start_span)[span_24](end_span).
* **Push to Clients:** Frames are streamed to all connected clients over WebSockets[span_25](start_span)[span_25](end_span).
* **Process Commands:** `process_command()` validates, clamps, and forwards instructions to `esp32_client.py`[span_26](start_span)[span_26](end_span).

---

## 🛡️ Safety Engineered Into Software
Two independent paths guarantee a controlled stop and prevent runaway motion[span_27](start_span)[span_27](end_span):
* **Emergency Stop:** Triggered manually by the operator or backend. Forces throttle to 0 and sends an immediate POST stop to the ESP32. Clearing requires an explicit disarm and re-arm sequence[span_28](start_span)[span_28](end_span).
* **Watchdog Fail-Safe:** Monitors time since the last valid command. If there is 1 second of silence, the vehicle halts automatically, and dropped WebSockets trigger it instantly[span_29](start_span)[span_29](end_span).

---

## 📊 Live Telemetry & Post-Race Analytics
* **Real-Time Monitoring:** Live tracking of speed, battery voltage, steering angle, sonar, LiDAR (TF-Luna), and IMU (MPU6050) readings[span_30](start_span)[span_30](end_span).
* **Post-Race Analytics:** Session history stored and queried from InfluxDB, visualizing key performance metrics (KPIs) such as lap times, top speed, average speed, sensor error rates, and total packet counts[span_31](start_span)[span_31](end_span)[span_32](start_span)[span_32](end_span)[span_33](start_span)[span_33](end_span).

---

## ✅ Validation & Results
* **Integration Tests:** 8/8 tests passed successfully[span_34](start_span)[span_34](end_span).
* **Success Criteria:** 5/6 criteria fully met[span_35](start_span)[span_35](end_span).
* **Fail-Safe Response:** < 1 second watchdog fail-safe activation[span_36](start_span)[span_36](end_span).
