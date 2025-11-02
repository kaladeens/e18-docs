# Troubleshooting

Common issues, causes, and quick fixes for the BushBot system.  
If problems persist, **repeat the full [Software Setup](software-setup.md)** process to ensure all dependencies and environment paths are correctly configured.

---

| 🧩 **Problem** | ⚙️ **Possible Cause** | 🧭 **Solution** |
|----------------|----------------------|----------------|
| **No video feed from payload** | Camera not detected or not initialised | Check ribbon cable connection, reboot the Pi, and confirm camera is enabled in `/boot/config.txt`. |
| **GUI fails to connect to payload** | Wrong IP address or hostname | Verify that you can ping the Pi (`ping bushbot.local` or `ping 192.168.x.x`). Update the host IP in the GUI or `constants.py`. |
| **No audio stream or missing sound** | Microphone not detected or device name mismatch | Confirm mic is connected. If using a new USB mic, edit `AUDIO_DEVICE_NAME` in `constants.py` to match the detected device (`arecord -l` to list). |
| **Audio delay or stuttering** | Network latency or buffer mismatch | Use a lower resolution or shorter buffer in the GStreamer pipeline; check Wi-Fi stability. |
| **No AI detections appear** | Model weights missing or wrong path | Verify `weights/` folder exists and that YOLOv8 and PANNs models were downloaded. |
| **`ModuleNotFoundError` or Python package missing** | Virtual environment not activated | Activate the venv before running the app: <br> `source venv/bin/activate` (Linux/macOS) or `.\venv\Scripts\activate` (Windows). |
| **PyGObject installation failed** | Not running in the correct terminal | Ensure commands are executed inside the **MSYS2 MinGW 64-bit** terminal, not PowerShell or CMD. Re-install following [Software Setup](software-setup.md). |
| **GStreamer not found / “not in PATH” error** | PATH variable not updated | Add `C:\msys64\mingw64\bin` to the **System Path** environment variable and restart your terminal or VS Code. |
| **`pkg-config` or compiler errors during install** | Incomplete MSYS2 setup | Re-run the MSYS2 setup and package installation section from [Software Setup](software-setup.md). |
| **GUI opens but window stays blank** | Missing dependencies or mismatch between Python version and PyQt | Ensure Python ≥3.11 and reinstall dependencies: `pip install -r requirements.txt`. |
| **Pi does not auto-reconnect after disconnect** | Network drop or static IP conflict | Ensure both host and Pi are on the same Wi-Fi. Restart both devices if necessary. |
| **Host or Pi logs show repeated reconnect attempts** | IP mismatch or hostname not resolving | Edit the IP in `constants.py` to match the Pi’s assigned address. |
| **Recording button unresponsive or missing outputs** | FFmpeg not installed correctly | Reinstall FFmpeg |
| **Executable fails to launch** | Missing system DLLs or incomplete PyInstaller build | Rebuild using the same environment with: <br> `pyinstaller --noconsole --icon=icons/in.ico gui_main.py`. |

---

!!! tip "General Fix"
If you encounter repeated issues with dependencies, terminal errors, or broken imports —  
**re-run the entire [Software Setup](software-setup.md)** guide step-by-step.  
Most problems are caused by skipped setup commands, wrong terminals, or missing environment variables.

---

!!! info "Quick Diagnostic Commands"
Use these commands to verify core components:

```bash
# Check connected audio devices
arecord -l

# Confirm camera module is detected
libcamera-hello --list-cameras

# Verify Python environment and path
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
