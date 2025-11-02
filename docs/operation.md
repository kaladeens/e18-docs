# Operation Guide

!!! info "Startup Information"
    Ensure that the **Raspberry Pi payload** is powered on and running the main application **before** attempting to connect.  
    On startup, the servo will automatically move to its **neutral (0°) position** — keep hands clear during this motion.

---

## Overview

The **BushBot Control GUI** provides a single interface to monitor, control, and analyse the system in real time.  
When launched, the application automatically connects to the Raspberry Pi payload and begins streaming **live video, audio, and telemetry** data.

The GUI integrates several major components:

**🎥 Video Feed**  
Displays real-time footage captured from the Pi camera.  

**🔊 Audio Stream**  
Plays sound directly from the onboard microphone.  

**🧠 AI Detection Modules**  
- Object detection via **YOLOv8**, which identifies and labels animals within the camera frame.  
- Environmental audio classification using **PANNs-inference**, which identifies animal calls and notable acoustic events.  

**📈 Telemetry Monitor**  
Displays live CPU usage, memory, and light-level data from the payload.  

**🎮 Control Interface**  
Allows full manual control of the payload, including camera tilt, IR filter, and optional base-movement controls if connected to a mobile platform.

---

## What You Can Do

### 1. View and Monitor

When the GUI launches, it automatically starts:

- **Live camera video** on the main display window  
- **Live audio playback** from the payload microphone  
- **Real-time telemetry updates** in the side panel  

Detected animals will appear as **bounding boxes** in the video feed with confidence scores, while **audio detections** (e.g., *frog*, *bat*, *duck*) will appear in the top-left overlay of the GUI.

---

### 2. Control the Payload

If the payload is mounted on a mobile base, motion can be controlled directly via keyboard or GUI controls:

| Key | Action |
|:--|:--|
| **W / A / S / D** | Move forward, left, backward, right |
| **Q / E** | Increase / Decrease maximum movement speed |
| **V** | Emergency stop |
| **R / F** | Adjust servo tilt (−45° to +45°) |
| **IR Filter Toggle** | Switch between filtered and unfiltered (manual mode) |

All control commands are sent asynchronously to ensure smooth, low-latency response.

---

### 3. IR Filter Modes

The onboard **infrared (IR) filter** improves image visibility and colour accuracy depending on lighting conditions.

**🌞 Normal Mode (Filter ON)**  
Used in bright daylight; the IR filter remains engaged to produce natural colour balance and prevent overexposure.

**🌙 IR Mode (Filter OFF)**  
Used in low-light or night-time conditions; the filter retracts, allowing infrared wavelengths to pass through — improving contrast and visibility in darkness.

**⚙️ Auto Mode**  
In **automatic mode**, the system continuously monitors light intensity from the onboard **LDR sensor**.  
When brightness drops below a defined threshold, the IR filter disengages automatically; when light increases, it re-engages.  
This ensures the best visibility without manual intervention.

Users can manually override this behaviour at any time via the GUI’s **IR Filter Toggle** control.

---

### 4. Interpret Data

- **Objects:** Green boxes with detected species labels (e.g., *kangaroo*, *bird*, *fox*)  
- **Audio Events:** Predicted sound class and confidence shown in GUI overlays  
- **Telemetry Panel:** Updates CPU, memory, and FPS values in real time  

These combined visual and audio cues allow for simultaneous observation of wildlife activity in the field.

---

### 5. Logging and Data Recording

Both the **Raspberry Pi** and **Host PC** maintain detailed runtime logs and data capture files to support analysis, troubleshooting, and experiment documentation.

**📜 Event Logging**  
- The system automatically records **key runtime events** such as:  
  - Payload connection and disconnection events  
  - Command transmissions from host to payload  
  - AI model initialisation and sensor startup  
  - Reconnect attempts and recovery notifications  
- Logs are timestamped and stored locally on both devices:
  - **Raspberry Pi:** `/home/pi/e18/rpi/logs/`  
  - **Host PC:** `e18/host/logs/`

**🎬 Data Recording Mode**  
The GUI provides a **Start Recording** button to capture a complete observation session:  
- Saves **synchronised video + audio** feed from the payload.  
- All **AI detections** (object and audio) are simultaneously written to a **CSV file**, including:
  - Timestamp  
  - Detection type (Object / Audio)  
  - Label (e.g., “kangaroo”, “birdsong”)  
  - Confidence score  
  - Frame index or audio chunk ID  
- Recordings are stored in the `e18/host/recordings/` folder with session-based timestamps (e.g., `session_2025-11-02_1430/`).

This allows each field session to be replayed, audited, and analysed offline — combining **multimodal data** (video, audio, detections, and system telemetry) in a single package.

---

## Summary

At a glance, the GUI provides:
- Instant access to live wildlife **video** and **audio**  
- Intelligent **object** and **sound recognition**  
- Configurable **IR filter** for day/night visibility  
- Full **manual control** of servo, filter, and movement  
- **Event logging** and **data recording** for full experiment traceability  
- Real-time **system telemetry**

BushBot is designed to be **autonomous, modular, and extendable**, enabling both real-time monitoring and comprehensive post-session analysis from a single unified interface.
