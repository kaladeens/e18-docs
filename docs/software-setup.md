# 🧩 Software Setup

This page explains how to install and configure all software components for both the **Raspberry Pi Zero 2 W** and the **Host Control PC**, preparing BushBot for its first run.

---

## 🧠 Raspberry Pi Setup

The Raspberry Pi controls onboard data acquisition, servo actuation, and AI inference.

---

### ⚙️ Step 1 – Flash the Project Image

1. Open **Raspberry Pi Imager** on your computer.  
2. Select:  
   - **Choose OS → Use Custom Image → bushbot.img**  
   - **Choose Storage →** your microSD card  
3. Click the ⚙️ **Advanced Options** icon and set:  
   - **Hostname:** `bushbot.local`  
   - **Enable SSH:** checked, with a password  
   - **Configure Wireless LAN:** enter SSID and password  
4. Click **Write** and wait until the process completes.  
5. Insert the card into the Pi and power it on.

!!! warning "First Boot Notice"
    The onboard LED will flash during startup.  
    **Do not disconnect power** until it stops flashing.

---

### 🔗 Step 2 – Clone the Repository

After connecting the Pi to Wi-Fi:

```bash
ssh admin@bushbot.local
sudo apt update && sudo apt install git -y
git clone git@github.com:kaladeens/e18.git
cd e18/rpi
```

---

### 🧰 Step 3 – Install Dependencies

Run the automated setup script:

```bash
chmod +x setup.sh
sudo ./setup.sh
```

> The installation takes several minutes and reboots automatically when finished.

---

### ▶️ Step 4 – Run the Main Application

After reboot:

```bash
ssh admin@bushbot.local
cd e18/rpi
./run.sh
```

The Pi now begins streaming video, audio, and telemetry to the host PC.

---

## 💻 Host Control PC Setup

The host computer runs the GUI, AI models, and data logging.

---

### 🧩 Step 1 – Environment Setup

```bash
cd e18/host
python -m venv venv
```

Activate the environment:

* **Windows**
  ```powershell
  .\venv\Scripts\activate
  ```
* **macOS / Linux**
  ```bash
  source venv/bin/activate
  ```

Install the required packages:

```bash
pip install -r requirements.txt
```

---

### 🪄 Step 2 – GStreamer (Windows Only)

1. Install [MSYS2](https://www.msys2.org/).  
2. Open **MSYS2 MSYS** and run:  
   ```bash
   pacman -Syu
   pacman -Su
   pacman -S mingw-w64-x86_64-toolchain mingw-w64-x86_64-gstreamer mingw-w64-x86_64-gst-plugins-base mingw-w64-x86_64-pkg-config
   ```
3. Launch *MINGW64* terminal → navigate to your project → activate venv → run:  
   ```bash
   pip install PyGObject
   ```
4. Add to System Path: `C:\msys64\mingw64\bin`  
5. Restart VS Code or your terminal.

---

### 🖥️ Step 3 – Launch the GUI

```bash
cd e18/host/bushbot_gui_v2
python gui_main.py
```

If configured correctly, the GUI will connect automatically and display live video and audio feeds.

---

### 🏗️ Step 4 – Optional Executable Build

```bash
pyinstaller --noconsole --icon=icons/in.ico gui_main.py
```

---

## 📦 Dependency Overview

| Library | Purpose |
|:--|:--|
| **aiohttp 3.12.15** | Async network communication |
| **av 15.1.0** | Audio/Video decoding |
| **numpy 2.2.6** | Core numerical operations |
| **opencv-python 4.12.0.88** | Computer vision |
| **PyQt6 6.9.1 / pyqt6-sip 13.10.2** | GUI framework |
| **requests 2.32.5** | HTTP requests |
| **sounddevice 0.5.2** | Microphone capture |
| **panns-inference 0.1.1** | Audio event detection |
| **ultralytics 8.3.204** | YOLO vision model |
| **torch / torchaudio / torchcodec** | Deep learning backend |
| **pandas 2.3.3** | Data logging and analysis |

---

## ⚖️ Licensing & Attribution

BushBot software is released under the **MIT License**.  
Third-party dependencies retain their respective open-source licenses (Apache 2.0, MIT, BSD).  
Ensure compliance when redistributing modified builds.

---

## 🚀 Next Steps
- [Operation](operation.md) – Daily usage & control modes  
- [Troubleshooting](troubleshooting.md) – Common issues and fixes  
- [Safety Manual](safety.md) – PPE & emergency response
