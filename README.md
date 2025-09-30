# H1 Motor Mount

This project provides firmware (`R1GIGA.ino`) for the Arduino GIGA R1 and a Python control app (`Talon_APP.py`) to operate a high-torque pan-tilt mount.  
The system is built to handle a **3.2 lb parabolic dish** for satellite tracking and hydrogen-line observation.

---

##  Features
- **Precise Motor Control**: NEMA 23 stepper motors with TB6600 drivers.  
- **Worm-Gear Reduction**: 30:1 and 60:1 ratios for smooth, accurate positioning.  
- **Cross-Platform Operation**: Arduino firmware + Python desktop/Raspberry Pi app.  
- **Satellite Tracking Ready**: Integration hooks for **Gpredict** and **Stellarium**.  
- **Safety & Telemetry**: Soft end-stops, angle feedback, and live serial reporting.  

---

##  Process Overview
1. **Arduino Firmware (`R1GIGA.ino`)**  
   - Handles motor step pulses, direction control, and safety limits.  
   - Provides telemetry (current azimuth/elevation) via serial connection.  

2. **Python App (`Talon_APP.py`)**  
   - Communicates with the Arduino over USB/Serial.  
   - Sends angle setpoints and reads feedback.  
   - Designed for integration with external tracking software (e.g., Gpredict).  

3. **User Control Flow**  
   - Upload Arduino firmware → Run Python app → Input angles or link to tracking software → Mount moves accordingly.  

---

##  Quick Start

### Arduino Firmware
1. Open `R1GIGA.ino` in the Arduino IDE.  
2. Select board: **GIGA R1 **.  
3. Flash to the Arduino GIGA R1.  

### Python Control App
```bash
python -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install pyserial
python Talon_APP.py
