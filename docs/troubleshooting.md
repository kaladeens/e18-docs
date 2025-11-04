# Troubleshooting

Common issues, causes, and quick fixes for the BushBot system.  
If problems persist, **repeat the full [Software Setup](software-setup.md)** process to ensure all dependencies and environment paths are correctly configured.

---

| 🧩 **Problem** | ⚙️ **Possible Cause** | 🧭 **Solution** |
|----------------|----------------------|----------------|
| **No video feed from payload** | Camera not detected or not initialised | Check ribbon cable connection, reboot the Pi, and confirm the camera is enabled in `/boot/config.txt`. |
| **GUI fails to connect to payload** | Wrong IP address or hostname | Verify connection with `ping bushbot.local` or `ping 192.168.x.x`. Update the host IP in the GUI or `constants.py`. |
| **No audio stream or missing sound** | Microphone not detected or incorrect device name | Check physical mic connection. If using a new USB mic, update `AUDIO_DEVICE_NAME` in `constants.py` to match the detected device (`arecord -l` to list). |
| **Audio delay or stuttering** | Network latency or buffer mismatch | Use a lower video resolution or shorter audio buffer in the GStreamer pipeline. Check Wi-Fi signal strength and stability. |
| **No AI detections appear** | Missing or incorrect model paths | Confirm the YOLOv11s and PANNs model weights are present in the `models/` folder. |
| **`ModuleNotFoundError` or package import failure** | Virtual environment not activated | Activate the venv before running: `source venv/bin/activate` (Linux/macOS) or `.\venv\Scripts\activate` (Windows). |
| **PyGObject installation failed** | Command executed in the wrong terminal | Run the setup inside the **MSYS2 MinGW 64-bit** terminal, not PowerShell or CMD. Then re-run [Software Setup](software-setup.md). |
| **GStreamer not found / “not in PATH”** | Environment variable not updated | Add `C:\msys64\mingw64\bin` to the Windows **System Path** and restart VS Code or your terminal. |
| **`pkg-config` or compiler errors** | Incomplete MSYS2 installation | Re-install build tools and packages following [Software Setup](software-setup.md). |
| **GUI opens but screen stays blank** | Dependency mismatch or missing Qt module | Ensure Python ≥3.11, then reinstall dependencies with `pip install -r requirements.txt`. |
| **Pi fails to reconnect after disconnect** | Network drop or static IP conflict | Make sure both host and Pi are on the same Wi-Fi network. Reboot both if necessary. |
| **Host or Pi logs show repeated reconnect attempts** | IP mismatch or DNS resolution error | Edit the host IP in `constants.py` to match the Pi’s assigned address. |
| **Recording button unresponsive or missing outputs** | FFmpeg not installed correctly | Reinstall **FFmpeg** for Windows (via MSYS2 or prebuilt binary) and restart the GUI. |
| **Executable fails to launch** | Missing DLLs or incomplete PyInstaller build | Rebuild the executable inside the venv: `pyinstaller --noconsole --icon=icons/in.ico gui_main.py`. |
| **Sensors or motors not responding** | Wiring or pin mismatch | Double-check all GPIO connections. Refer to [electronics.md](electronics.md) for correct pin assignments. |
| **Camera or LDR not working properly** | Incorrect SPI or CSI wiring | Inspect the camera ribbon orientation and ADC wiring for the LDR. Refer to [electronics.md](electronics.md) for the exact SPI pin map. |

---

!!! tip "General Fix"
If you encounter repeated errors, dependency issues, or broken imports —  
**re-run the entire [Software Setup](software-setup.md)** guide step-by-step.  
Most problems are caused by skipped commands, incorrect terminals, or missing environment variables.

---

!!! info "Quick Diagnostic Commands"

**On the Raspberry Pi**
```bash
source /path/to/rpi/venv/bin/activate
python utils.py  # Run system diagnostics

# List connected audio devices
arecord -l

# Confirm camera module
libcamera-hello --list-cameras

# Verify Python environment
python --version
which python

# Test if GUI can reach payload
ping bushbot.local
```

---

!!! warning "Rebuilding MSYS2 Environment (Windows Only)"
If GStreamer or PyGObject installation fails repeatedly,  
open **MSYS2 MSYS** terminal and reset the environment:

```bash
pacman -Syu
pacman -S mingw-w64-x86_64-toolchain \
          mingw-w64-x86_64-gstreamer \
          mingw-w64-x86_64-gst-plugins-base \
          mingw-w64-x86_64-pkg-config
```

Then open **MSYS2 MinGW 64-bit** terminal and reinstall Python bindings:

```bash
source venv/Scripts/activate
pip install PyGObject
```

After completing these steps, repeat **Section 2 – GStreamer Installation** in [Software Setup](software-setup.md).