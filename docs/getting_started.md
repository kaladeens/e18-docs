# Getting Started

This guide walks you through setting up both the **Raspberry Pi Zero 2W** and the **Host Control PC** to bring BushBot online for the first time.

!!! info "Before You Begin"

    ### ⚠️ Safety First
    BushBot contains **powered servos and live electronics** — handle with care.  
    Before operating:
    - Inspect all cables, connectors, and the power supply for wear or damage.  
    - Ensure the workspace is clear of liquids, debris, or metallic objects.  
    - Review the full [Safety Manual](safety.md) for PPE, electrical precautions, and shutdown procedures.  

    ### 🧰 What You’ll Need
    - 1× Raspberry Pi Zero 2W (pre-flashed with the **BushBot project image**)  
    - 12 V DC regulated power supply (≥ 2 A)  
    - Host PC with Python 3.11 + Git installed  
    - Stable Wi-Fi network (2.4 GHz recommended)  

    ### 💡 Quick Tip
    Keep the system tethered with a long wire to avoid tripping hazards.

---

## Configuration Overview

BushBot can be operated in two main configurations:

| Configuration | Description |
|----------------|-------------|
| **Sensor Payload Only** | Operates the camera, microphone, IR filter, and tilt mechanism independently. Suitable for bench testing and AI model verification. |
| **Full BushBot Robot** | Integrates the payload with the R03 robotic base for mobile field testing. Includes additional connections for chassis power and communication. |

Regardless of which version you received, **the setup process for the payload remains identical**.  
Only the power routing and connector layout differ slightly when integrated into the robotic base.

---

## Powering On

To start up BushBot:

1. Connect the **12 V barrel plug** from the regulated power supply to the payload’s power jack.  
2. Verify that you have adequate tether length for safe operation.  
3. Wait for the **onboard Pi LED** to finish flashing — this indicates the boot process is complete.  

<p align="center">
  <img src="/assets/led_pi0.jpg" alt="Raspberry Pi Zero 2W LED indicator" width="65%">
</p>
<p align="center">
  <em>Figure 1. Raspberry Pi Zero 2W — wait until the onboard LED stops flashing before proceeding.</em>
</p>

!!! warning "Servo Safety Notice"

    On startup, BushBot will **automatically initialise the servo to its neutral (0°) position**, aligning the camera to face forward.  
    - **Keep your hands clear** of the camera assembly and tilt mechanism during power-up.  
    - Ensure no cables or objects obstruct the servo arm’s path.  
    - Wait until the servo finishes movement before handling the payload.

---

!!! note "Automatic Wi-Fi Connection"

    When flashing the **BushBot project image** using **Raspberry Pi Imager**, you can still configure Wi-Fi for automatic connection:  
    1. Open **Raspberry Pi Imager** → Select *Choose OS* → *Use Custom Image* → select the `bushbot.img` file.  
    2. Click the ⚙️ **Advanced Options** icon and set:
        - **Wireless LAN SSID** and **Password**  
        - **Hostname** (e.g., `bushbot.local`) for SSH access  
        - Optionally enable **SSH** and set a password (recommended).  
    3. Write the image to your microSD card and insert it into the Pi.  

<style>
  .image-row {
    display: flex;
    justify-content: center;
    gap: 30px;
    flex-wrap: wrap;
    align-items: flex-start;
    margin-top: 10px;
    margin-bottom: 10px;
  }
  .image-row figure {
    flex: 1 1 45%;
    text-align: center;
    margin: 0;
  }
  .image-row img {
    width: 100%;
    max-width: 450px;
    border-radius: 6px;
    box-shadow: 0 2px 6px rgba(0,0,0,0.1);
  }
  .image-row figcaption {
    font-style: italic;
    font-size: 0.9em;
    margin-top: 5px;
  }
</style>

<div class="image-row">
  <figure>
    <img src="/assets/custom_rpi.png" alt="Raspberry Pi Imager Wi-Fi configuration">
    <figcaption>Figure 2. Configuring Wi-Fi credentials and hostname in Raspberry Pi Imager’s advanced options.</figcaption>
  </figure>
  <figure>
    <img src="/assets/pi_imager.png" alt="Raspberry Pi Imager overview">
    <figcaption>Figure 3. The Raspberry Pi Imager interface with a custom image selected for BushBot.</figcaption>
  </figure>
</div>


On first boot, the Pi will automatically join your configured Wi-Fi network.  
You can then connect from the host PC using:
```
ssh admin@bushbot.local
```

---

## Initial Software Launch

Once both the **Raspberry Pi** and **Host PC** are powered and connected:

1. **On the Pi** (via SSH or terminal):  
    Navigate to the project folder and run the setup script to install dependencies and configure services.  
    ```
    cd ~/e18/rpi
    ./setup.sh
    ```

2. **On the Host PC:**  
    Launch the graphical control interface.  
    ```
    cd ~/e18/host/bushbot_gui_v2
    python gui_main.py
    ```

If both systems are configured correctly (as described in [Software Setup](software-setup.md)), the GUI will display live video and audio streams once the Pi begins broadcasting.  

---

## Next Steps
- For detailed dependency setup, see [Software Setup](software-setup.md).  
- For troubleshooting connection or power issues, see [Troubleshooting](troubleshooting.md).  
- For daily use and operational modes, see [Operation](operation.md).
