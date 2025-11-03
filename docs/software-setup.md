# Software Setup

!!! danger "⚠️ Critical Setup Notice"
    This guide includes several **interdependent steps** that must be followed **in order**.  
    Skipping or performing steps incorrectly (e.g., not using the right terminal, or forgetting to activate your Python virtual environment) will cause the **build to fail** or the **GUI to malfunction**.  

    ✅ **Before starting, ensure that:**
    - You have installed **Python 3.11 or newer**  
    - You are connected to a **stable Wi-Fi network**  
    - You have the **BushBot repository cloned** or available via GitHub  
    - You have **admin privileges** to install packages on both systems  

This section explains how to install and configure the software for both the **Raspberry Pi Zero 2 W** and the **Host Control PC**, preparing BushBot for its first run.

---

## Raspberry Pi Setup

The Raspberry Pi handles onboard data acquisition, servo actuation, and communication with the host.

---

### Step 1 – Flash the Project Image

The BushBot image is built on **Raspberry Pi OS Lite (Bookworm, 32-bit)**, optimised for the limited memory of the Pi Zero 2 W.  
You do not need to install a separate operating system — simply write the provided image file (`bushbot.img`) using **Raspberry Pi Imager**.

Select the following options:  
 • **Choose OS → Use Custom Image → `bushbot.img`**  
 • **Choose Storage →** your microSD card  

Click the ⚙️ **Advanced Options** icon and set:  
 • **Hostname:** `bushbot.local`  
 • **Enable SSH:** ✔ (checked) with a password  
 • **Configure Wireless LAN:** enter your **SSID** and **password**  

Click **Write**, then wait until the process completes.  

Safely eject the card, insert it into the Pi, and power on.


!!! warning "First Boot Notice"
    The onboard LED will flash during startup.  
    **Do not disconnect power** until it stops flashing.

---
### Step 2 – Clone the Repository

Most **BushBot project images** come with the repository and dependencies already pre-installed.  
However, if you are using a fresh or custom image, follow these steps to clone the project manually.

!!! note "When to Use This Step"
    You only need to perform this step if the `e18` folder is **not** already present in your home directory on the Pi.

To connect and clone the repository:

```
ssh {username}@bushbot.local
sudo apt update && sudo apt install git -y
git clone git@github.com:kaladeens/e18.git
cd /path/to/e18/rpi
```

---

### Step 3 – Install Dependencies

The default **BushBot image** includes all required packages and services.  
If you are using a minimal or custom build, you can manually install all dependencies using the setup script below:

```
chmod +x setup.sh
sudo ./setup.sh
```

!!! info "Setup Duration"
    Installation may take several minutes and will automatically reboot the Pi when complete.

### Step 4 – Run the Main Application

After reboot:

```
# connect from your pc 
ssh {username}@bushbot.local
cd /path/to/e18/rpi
chmod +x run.sh
sudo ./run.sh
```

The Pi now begins streaming video, audio, and telemetry to the host PC.

!!! danger "Network Handling"
    On first boot, the Pi will attempt to establish a **self-assigned static IP** to maintain a stable connection with the host system.  
    This is designed to make the network link more robust and prevent address conflicts during field operation.  

    In rare cases where the static IP overlaps with another device on the network, the connection may fail.  
    If this occurs, you can modify the assigned address in the **host-side configuration file** before launching the GUI.
---

## Host Control PC Setup

The host computer runs the GUI, AI models, and data logging.

---
!!! tip "Using the Prebuilt Executable"

    If you only plan to **run** BushBot and not modify the source code, you can skip the build steps.

    - Simply use the **prebuilt executable** (`bushbot_gui.exe`) provided with the release package.
    - No Python, MSYS2, or GStreamer setup is required for normal operation.


### Step 1 – Create and Activate a Virtual Environment

```
cd /path/to/e18/host
python -m venv venv
```

Activate the environment:

* **Windows**
  ```powershell
  .\venv\Scripts\activate
  ```
* **macOS / Linux**
  ```
  source venv/bin/activate
  ```

Install required packages:

```
pip install -r requirements.txt
```

---

### Step 2 – Install GStreamer (Windows Only)

!!! danger "⚠️ GStreamer Setup – High Failure Risk"
    This section must be done **exactly as described** — using the wrong terminal or skipping environment activation will **break the GUI build**.  
    - Use the **MSYS2 MinGW 64-bit** terminal (**purple prompt**, starts with `MINGW64`), not PowerShell or CMD, when specified.  
    - Ensure your **virtual environment is activated** (`(venv)` visible before your prompt).  
    - If you miss either condition, **PyGObject will not install**, and video streaming will fail.

---

#### Install MSYS2 and Update Core Packages

- Download and install **[MSYS2](https://www.msys2.org/)** (use default settings).  
- Open the **MSYS2 MSYS** terminal (blue icon in Start Menu).  
- Run the following commands one at a time:

```
pacman -Syu
# If prompted, close and reopen the terminal, then:
pacman -Su
```

---

#### Install GStreamer and Build Tools

In the **MSYS2 MSYS** terminal, install all required components:

```
pacman -S mingw-w64-x86_64-toolchain \
          mingw-w64-x86_64-gstreamer \
          mingw-w64-x86_64-gst-plugins-base \
          mingw-w64-x86_64-pkg-config \
          mingw-w64-x86_64-ffmpeg
```

!!! info "Why FFmpeg?"
    FFmpeg provides codecs and media processing utilities required by GStreamer, OpenCV, and PyAV.  
    Without it, the GUI may fail to play audio/video or save recordings properly.

---

#### Build Python Bindings

- Open the **MSYS2 MinGW 64-bit** terminal (purple icon).  
- Navigate to your project directory and activate your virtual environment:

```
cd /c/Users/YourUsername/path/to/e18/host
source venv/Scripts/activate
pip install PyGObject
```

!!! warning
    Ensure `(venv)` appears before your prompt — if not, stop and rerun the activation command.  
    PyGObject **will fail to install** if this step is skipped or run in the wrong terminal.

---

#### Configure System Path

Add the GStreamer binaries to your Windows PATH:

```
C:\msys64\mingw64\bin
```

**Steps:**
- Open **Control Panel → System → Advanced System Settings → Environment Variables**  
- Under **System Variables**, select `Path → Edit → New`  
- Paste the above path and click **OK**  
- Restart VS Code or any open terminals.

---

#### Verify the Installation

Run the following command to confirm the binding works:

```
python -c "import gi; print('PyGObject OK')"
```

If the message `PyGObject OK` appears, your setup was successful.

### Step 3 – Launch the GUI

```
cd /path/to/e18/host/bushbot_gui_v2
python gui_main.py
```

If configured correctly, the GUI connects automatically and displays live video and audio feeds.

---

<!-- Two-column dependency tables (responsive) -->
<style>
  .deps-grid{
    display:grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 1rem;
  }
  .deps-grid h3{ margin: .25rem 0 .5rem; }
  .deps-grid table{ width:100%; border-collapse:collapse; }
  .deps-grid th, .deps-grid td{
    padding:6px 10px;
    border-bottom:1px solid var(--md-typeset-table-color, #e0e0e0);
    vertical-align: top;
  }
  .deps-grid thead th{ text-align:left; }
</style>

<div class="deps-grid">

  <div>
    <h3>💻 Host Control PC Dependencies</h3>
    <table>
      <thead>
        <tr><th>Library</th><th>Purpose</th></tr>
      </thead>
      <tbody>
        <tr><td><strong>aiohttp 3.12.15</strong></td><td>Async network communication</td></tr>
        <tr><td><strong>opencv-python 4.12.0.88</strong></td><td>Computer vision</td></tr>
        <tr><td><strong>PyQt6 6.9.1 / pyqt6-sip 13.10.2</strong></td><td>GUI framework</td></tr>
        <tr><td><strong>requests 2.32.5</strong></td><td>HTTP requests</td></tr>
        <tr><td><strong>sounddevice 0.5.2</strong></td><td>Audio output</td></tr>
        <tr><td><strong>panns-inference 0.1.1</strong></td><td>Audio event detection</td></tr>
        <tr><td><strong>ultralytics 8.3.204</strong></td><td>YOLO vision model</td></tr>
        <tr><td><strong>torch / torchaudio / torchcodec</strong></td><td>Deep learning backend</td></tr>
        <tr><td><strong>pandas 2.3.3</strong></td><td>Data logging and analysis</td></tr>
      </tbody>
    </table>
  </div>

  <div>
    <h3>🐧 Raspberry Pi Payload Dependencies</h3>
    <table>
      <thead>
        <tr><th>Library</th><th>Purpose</th></tr>
      </thead>
      <tbody>
        <tr><td><strong>av 11.0.0</strong></td><td>Lightweight video decoding for Pi</td></tr>
        <tr><td><strong>gpiozero 2.0.1</strong></td><td>GPIO control for servos and peripherals</td></tr>
        <tr><td><strong>numpy &lt;2</strong></td><td>Core numerical operations</td></tr>
        <tr><td><strong>picamera2 0.3.30</strong></td><td>Camera interface and streaming</td></tr>
        <tr><td><strong>psutil 7.0.0</strong></td><td>System resource monitoring</td></tr>
        <tr><td><strong>sounddevice 0.5.2</strong></td><td>Audio capture for Pi mic</td></tr>
        <tr><td><strong>spidev 3.7</strong></td><td>SPI communication for sensors</td></tr>
      </tbody>
    </table>
  </div>

</div>



## Licensing and Attribution

BushBot software is released under the **MIT License**.  
Third-party dependencies retain their respective open-source licenses (Apache 2.0, MIT, BSD).  
Ensure compliance when redistributing modified builds.

---

## Next Steps
- [Operation](operation.md) – Daily usage & control modes  
- [Troubleshooting](troubleshooting.md) – Common issues and fixes  
- [Safety Manual](safety.md) – PPE & emergency response
