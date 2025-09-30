# H1 Motor Mount

This repository contains firmware (`R1GIGA.ino`) for the Arduino GIGA R1 and a Python control app (`Talon_APP.py`) for operating a high-torque pan-tilt mount.  
The system is designed to drive a **3.2 lb parabolic dish** for **satellite tracking** and **hydrogen-line radio astronomy**, with precision stepper motors, worm-gear reduction, and a fully integrated software stack.

---

# 🔧 Arduino Firmware — `R1GIGA.ino`

##  Overview
The firmware (v4.3, codename **TALON — Tactical Acquisition & Lock-On Network**) transforms the Arduino GIGA R1 into the low-level motor control brain of the mount.  
It is responsible for **precise motor stepping**, **UI feedback**, **serial communications**, and **safety enforcement**.

###  Key Features
- **Animated Splash Screen**: Multi-stage intro with grid fade, text reveal, and scanning animation.  
- **Direct Hardware Timer Control (TIM3)**: Motor stepping handled at register level for smooth, jitter-free motion.  
- **Touchscreen UI**: Correctly mapped screen rotation and touch controls for jog buttons.  
- **Re-Tuned Motion Profile**: Acceleration and velocity profiles optimized for stability and responsiveness.  
- **Diagnostic Display**: Shows active command source (`SERIAL`, `JOG`, `IDLE`).  
- **Theme Options**: Switch between light and “Dark Aerospace” UI by toggling `USE_DARK_THEME`.  
- **Full Serial Command Set**: All listed commands implemented (see below).  

---

##  Processes
1. **Boot** → Play splash screen animation.  
2. **Initialize** → Configure hardware timer, drivers, and UI.  
3. **Command Loop** → Poll for jog/touchscreen input and serial commands.  
4. **Motor Control** → Step pulses generated via TIM3 ISR at fixed frequency.  
5. **Telemetry** → Current azimuth/elevation continuously reported over serial.  

---

##  Supported Serial Commands 
- `SET_AZ <angle>` → Move to absolute azimuth angle.  
- `SET_EL <angle>` → Move to absolute elevation angle.  
- `JOG_AZ <+/-steps>` → Relative jog in azimuth.  
- `JOG_EL <+/-steps>` → Relative jog in elevation.  
- `STOP` → Immediately halt all motor motion.     

---

##  Quick Start (Arduino)
1. Open `R1GIGA.ino` in Arduino IDE.  
2. Select **Arduino GIGA R1** as the board.  
3. Compile and flash to your device.  
4. Connect stepper drivers and power supply (match pinouts in firmware).  

---

##  Troubleshooting (Arduino)
- **Motors stutter or skip** → Check TB6600 DIP switch settings (current, microstepping).  
- **Touchscreen unresponsive** → Ensure screen rotation constant matches your hardware.  
- **Firmware unbuildable** → Use latest Arduino IDE + board manager for GIGA R1.  
- **Serial commands ignored** → Confirm baud rate (`115200`) matches both ends.  

---

---

#  Python Control App — `Talon_APP.py`

##  Overview
The Python application provides a **graphical interface**, **radar-style visualization**, and **telemetry logging** for the motor mount. It connects via serial to the Arduino and optionally integrates with **Stellarium’s Remote Control API and GPredicts rotor control**.

###  Key Features
- **Custom Theme System**: Dark military-inspired UI with radar sweep, scanlines, and color-coded elements.  
- **Tkinter GUI**: Lightweight cross-platform app with terminal/log pane and radar canvas.  
- **Radar Sweep Visualization**: Displays dish position (“ACTUAL”) and target (“TARGET”) with animated sweep.  
- **Telemetry Integration**: Serial feed of azimuth/elevation displayed in UI.  
- **Stellarium API Support**: Can fetch celestial targets directly from Stellarium.  
- **Background Effects**: Randomized stars/noise to simulate sky background.  

---

##  Processes
1. **Initialize UI** → Load theme, create Tkinter windows, logs, radar canvas.  
2. **Serial Connect** → Detect Arduino GIGA R1, open port, sync baud rate.  
3. **Main Loop** → Poll for Arduino telemetry and Stellarium targets.  
4. **UI Update** → Update radar sweep, logs, and telemetry text in real time.  
5. **User Input** → Jog dish, set angles, or hand control to Stellarium feed.  

---

## ⚡ Quick Start (Python)
```bash
python -m venv .venv
source .venv/bin/activate     # Windows: .venv\Scripts\activate
pip install pyserial requests
python Talon_APP.py
