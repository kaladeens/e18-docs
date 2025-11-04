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
| Field operation | Enclosed shoes, gloves | Protects from rough terrain and fauna |
| Soldering / prototyping | Safety glasses, fume extraction mask | Use in a ventilated area only |

!!! warning
    Never operate the system near open liquids, flammable materials, or in heavy rain unless the payload is safely enclosed and waterproofed.

---

## 3. Pre-Use Safety Checklist

| Item | Action | Confirm |
|------|---------|----------|
| Power cables | Inspect for frayed or exposed wires | ☐ |
| Camera and servo mounts | Secure and aligned | ☐ |
| Grounding | Confirm all modules share a common GND | ☐ |
| Firmware | Confirm latest version installed | ☐ |
| Wi-Fi link | Test connection to host PC | ☐ |
| Environment | Ensure workspace is clear, stable, and dry | ☐ |

!!! info
    Complete this checklist before every session.  
    Record any faults or irregularities before operation to ask for support.

---

## 4. Electrical Safety and Handling

- Always work on an **electrostatic-safe mat** when connecting or disconnecting hardware.  
- **Ground yourself** using an antistatic wrist strap before handling the Raspberry Pi, camera, or sensor boards.  
- **Disconnect power** before wiring, changing modules, or attaching the servo.  
- Double-check all power and ground wiring for **continuity** before powering on to prevent shorts.  
- Avoid touching connectors or exposed pins while powered.  
- Do not overload GPIO pins — check the pinout (see [Electronics](electronics.md)) before connecting.  
- Keep liquids, tools, and metallic debris away from live circuits.  
- Use only the supplied **12 V DC ± 5 % regulated power supply**.  
- Never power the payload simultaneously via USB and barrel jack.  
- Ensure all wiring is enclosed and secured on the **Vero board**. Wires should be neatly tucked and insulated to prevent accidental contact with power rails.

<p align="center">
  <img src="/assets/veroboard.png" alt="Example of safe Vero board wiring layout" width="600"/>
  <br>
  <em>Figure 1. Payload Vero board with safely routed and enclosed wiring</em>
</p>

!!! tip
    Keep your workspace clear. A misplaced screwdriver or loose wire can short terminals and permanently damage the board.

---

## 5. Safe Startup Procedure

1. Verify the **12 V supply voltage** using a multimeter before connecting.  
2. Plug the power lead into the barrel plug **before** turning on the host PC.  
3. Wait ~30 seconds for the Pi to fully boot.  
4. Launch the host GUI (see [Operation](operation.md)).  
5. Confirm that **video** and **telemetry** are active.

!!! info
    The **green LED** on the payload indicates the system is powered and running.
    If any issues occur see [Troubleshooting](troubleshooting.md).
---

## 6. Normal Operation Guidelines

- Do not manually move the camera assembly while powered — use GUI tilt controls only.  
- Limit continuous operation to **2 hours**, followed by a **10-minute cooldown**.  
- Keep cables tidy and away from moving servo arms.  
- During field tests, ensure the chassis is stable and not on a slope or edge.  

---

## 7. Shutdown Procedure

1. In the GUI, stop all active streams or data collection.  
2. SSH into the Pi and execute:
```bash
sudo shutdown now
```
3. Once the Pi’s green LED turns off, disconnect the power lead.  
4. Power off the host PC if testing is complete.  

!!! warning
      Always disconnect the 12 V line **last** to prevent back-feeding the regulator.

---

## 8. Emergency and Fault Response

**Smoke, sparks, or overheating:**  
- Immediately disconnect power at the source.  
- Do **not** touch the system until all capacitors discharge.  
- Report the fault and label the hardware as *out of service*.  

**Electrical shock:**  
- Cut power immediately.  
- Seek medical attention and report to supervisor.  

**Firmware freeze or unresponsive hardware:**  
- Stop all processes via SSH (`pkill python3`).  
- Power cycle only after at least **10 seconds** off time.  

---

## 9. Maintenance

- Inspect wiring and connectors weekly for loose or corroded contacts.  
- Reapply thermal compound on regulators and CPU every 6 months if used regularly.  
- Keep firmware and dependencies updated via Git pull or `apt update`.  
- Store electronics in **dry, antistatic packaging** when not in use.  

!!! tip
      Preventive maintenance reduces faults and ensures reliable operation in both lab and field settings.