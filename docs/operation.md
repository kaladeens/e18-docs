# Operation Guide

!!! info "Startup Information"
    Make sure the **Raspberry Pi payload** is powered on and running the main app **before** connecting.  
    When it starts up, the servo moves to its **neutral (0°) position** — keep hands clear while it does.

---

## Overview

The **BushBot Control GUI** is your main dashboard for watching, controlling, and recording the system in real time.  
Once opened, it automatically connects to the Raspberry Pi and starts streaming **video, audio, and telemetry**.

Main features include:

**🎥 Video Feed**  
Shows the live camera view from the Pi.

**🔊 Audio Stream**  
Plays live audio from the USB microphone.

**🧠 AI Detection**  
- **YOLOv11s** for spotting animals on video with bounding boxes and confidence scores.  
- **PANNs-Inference ensemble** (five models combined) for identifying animal sounds.

**📈 Telemetry**  
Displays live **CPU**, **memory**, **FPS**, and **light level (LDR)** readings from the payload.

**🎮 Controls**  
Lets you adjust **camera tilt**, switch **IR filter modes**, and move the base if it’s connected.

---

## What You Can Do

### 1. Watch and Listen

When the GUI opens, it automatically starts:

- The **live camera feed**  
- **Audio playback** from the payload  
- **Telemetry updates** showing CPU, memory, FPS, and LDR readings  

Detected animals appear as boxes with labels and confidence scores.  
Detected sounds (like *kangaroo*, *bat*, or *snake*) pop up in the top corner of the window.

If the connection drops, the GUI will **try to reconnect automatically** — no restart needed.

---

### 2. Control the Payload

Use either the on-screen buttons or your keyboard to control movement and camera tilt.

| Key | Action |
|:--|:--|
| **W / A / S / D** | Move forward, left, backward, right |
| **Q / E or GUI speed slider** | Increase / Decrease motor speed |
| **V** | Emergency stop |
| **R / F** | Tilt camera up / down (−30° to +30°) |
| **IR Filter Toggle** | Switch filter mode manually |

All controls respond quickly with minimal delay.

**Dead-man switch:**  
The robot only moves while you hold a key.  
Letting go stops motion instantly, keeping it safe and easy to handle.

---

!!! tip "Manual Command Input"
    These commands can also be sent manually through the text box on the right side of the GUI.  
    This is handy for quick testing or debugging individual functions.

```
stop        – immediately stop all motors  
set_servo   – set the camera servo angle  
set_v       – set left and right motor speeds  
ir_mode     – switch IR filter mode (on / off / auto)  
status      – request a system status update  
get         – retrieve sensor data (e.g., LDR value)
```

---

### 3. IR Filter Modes

The **IR filter** helps the camera adapt to lighting:

- **🌞 Normal Mode** – Filter on, for bright daylight and true colours.  
- **🌙 IR Mode** – Filter off, for better vision in low light.  
- **⚙️ Auto Mode** – Switches automatically based on the **LDR sensor** brightness.  

You can change modes anytime through the GUI.

---

### 4. Reading the Data

- **Visual detections:** Animal name and confidence score shown on-screen.  
- **Audio detections:** Label and confidence displayed in the top-right corner.  
- **Telemetry:** CPU, memory, FPS, and LDR readings update live.  
- **Logs:** Connection and transmission messages appear in the GUI log window — but these are **not saved** to a file.  
  They’re displayed only during runtime to help monitor communication and system status.

This gives a complete view of what the system sees, hears, and reports in real time.

---

### 5. Recording Sessions

Press **Start Data Collection** to begin a recording session.  
Press **Stop Data Collection** when you’re done.

When recording is active:
- **Video and audio** streams are captured together.  
- **AI detections** (from YOLOv11s and PANNs) are written to a **CSV file**, including:  
  - Timestamp  
  - Detection type (Object / Audio)  
  - Label  
  - Confidence score  
  - Frame index or audio segment ID  

When you stop recording, the files are saved automatically in:  
`e18/host/recordings/session_YYYY-MM-DD_HHMM/`

Each session folder contains:
- Recorded **video (.mp4)**  
- Recorded **audio (.wav)**  
- **Detections CSV log** with all identified species and confidence values  

You can replay, review, or analyse these sessions later for reports or testing documentation.

---

## Summary

The BushBot GUI lets you:

- Watch and listen to **live feeds**  
- See real-time **animal detections** and confidence levels  
- Use simple, responsive **controls** with built-in safety  
- Adjust or automate the **IR filter**  
- View **system stats** like CPU, memory, FPS, and light level  
- Track **live logs** in the GUI window (not saved to file)  
- Record sessions with **video, audio, and detection logs** saved for later analysis  

BushBot is built to be clear, responsive, and reliable — perfect for both real-time monitoring and post-session review.
