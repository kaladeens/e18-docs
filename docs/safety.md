# Safety Manual and Standard Operating Procedure (SOP)

This document outlines the **safe operation, maintenance, and emergency procedures** for the BushBot payload and host system.  
All users must read this section before operating or handling the system.

---

## 1. Purpose
To ensure safe, consistent, and responsible use of the BushBot system in laboratory and field environments while minimising risk to users, equipment, and wildlife.

---

## 2. Personal Protective Equipment (PPE)

| Task | Required PPE | Notes |
|------|---------------|-------|
| Hardware assembly | Safety glasses, antistatic wrist strap | Protects eyes and electronics |
| Electrical testing | Safety glasses, insulated gloves | Avoid short circuits during live testing |
| Field operation | Enclosed shoes, gloves | Protection from terrain and fauna |
| Soldering / prototyping | Safety glasses, fume extraction mask | Use in ventilated area only |

> ⚠️ **Never operate the system near open liquids, flammable materials, or in heavy rain.**

---

## 3. Pre-Use Safety Checklist

| Item | Action | Confirm |
|------|---------|----------|
| Power cables | Inspect for frayed or exposed wires | ☐ |
| Camera and servo mounts | Secure and aligned | ☐ |
| Grounding | Confirm all metal frames are grounded | ☐ |
| Firmware | Confirm latest firmware version installed | ☐ |
| Wi-Fi | Test connection stability to host PC | ☐ |
| Environment | Clear of obstacles, dry surface | ☐ |

> Complete this checklist before every session. Document any faults in the logbook.

---

## 4. Safe Startup Procedure

1. Verify power supply voltage: **12 V DC ±5%**.  
2. Connect tethered power to regulator before turning on host PC.  
3. Power on the Raspberry Pi via the switch.  
4. Launch the host GUI only after the Pi has booted (see [Operation](operation.md)).  
5. Confirm video and telemetry links are live.

> 🚦 The green LED on the payload indicates active status.

---

## 5. Normal Operation Guidelines

- Do not physically move the camera assembly while powered; use GUI tilt controls.
- Maintain clear line-of-sight for the Wi-Fi link between Pi and host.
- Limit continuous operation to ≤ 2 hours before a 10-minute cooldown to prevent thermal buildup.
- Observe the **system temperature indicator** in the GUI; shut down if it exceeds **65 °C**.

---

## 6. Shutdown Procedure

1. From the GUI, click **Stop Stream** to end all processes.  
2. SSH into the Pi and execute:
   ```bash
   sudo shutdown now
