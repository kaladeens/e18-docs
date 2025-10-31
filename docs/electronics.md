# Electronics

The BushBot’s electronics system handles **power regulation**, **signal routing**, and **actuation**.

---

## Power Supply

| Source | Voltage | Destination |
|--------|----------|-------------|
| Tethered supply | 12V | Voltage regulator |
| Regulated output | 5V | Raspberry Pi |
| Regulated output | 3.3V | Microphone & sensors |

> ⚠️ Ensure polarity is correct before powering. Reverse polarity may damage components.

---

## Signal Connections

| Component | GPIO Pin | Description |
|------------|-----------|-------------|
| Servo | GPIO 17 | PWM control |
| Microphone | GPIO 21 | Audio input |
| Light Sensor | GPIO 4 | Brightness detection |
| Camera | CSI | Video capture |

Use consistent wire colors (e.g., red = +V, black = GND, yellow = signal) for safety.


---
# Hardware Setup

This section explains how to assemble and wire the **BushBot payload**.

---

## Components
- Raspberry Pi Zero 2W
- Camera Module (IR-assisted)
- Microphone module
- Servo motor (±30° camera tilt)
- Power regulation board (12V → 5V/3.3V)
- Wi-Fi communication interface

---

## Assembly Steps

1. **Mount the camera** securely on the front bracket.  
2. **Connect the servo motor** to the camera gimbal using M2 screws.  
3. **Wire the peripherals**:
   - Camera ribbon → Pi camera port
   - Microphone → GPIO pins
   - Servo signal → PWM GPIO
   - Power input → voltage regulator
4. **Secure the payload** to the R03 robot chassis using designated mounting points.
