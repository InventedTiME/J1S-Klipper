# J1S-Klipper
Innocube3D IDEX conversion kit application for the J1S.

# Snapmaker J1S → Klipper IDEX Conversion

## Reclaiming control of my J1S by swapping out its locked-down firmware for a fully open, Klipper-powered IDEX setup with the Innocube3D IDEX Conversion kit.

Available here.... https://www.innocube3d.com/products/tenlog-3d-printer-klipper-upgrade-kit

ChatGPT, Claude, Github Copilot, NotebookLM and Perplexity (running several different models) are helping me document and tweak the plan going forward.
---

## 🚀 What’s This?

General plan for conversion:

1. Wiring up the IDEX kit  
2. Flashing Klipper and companion firmware  
3. Configuring dual-extruder IDEX in Klipper  
4. Getting prints running better than ever  

---

## 🛠️ What You’ll Need

- **Snapmaker J1S** (2021 model or later)  
- **Innocube3D IDEX Klipper Conversion Kit**  
  - Dual stepper drivers  
  - Fans, thermistors, wiring harness, connectors  
- **Raspberry Pi 4** (or equivalent SBC) with:  
  - Raspberry Pi OS (64-bit)  
  - SSH & Wi-Fi access  
- **MicroSD card** (≥16 GB) for Pi and separate one for the J1S board  
- **USB-A → USB-A cable** (to connect Pi to printer board)  
- **Basic hand tools**: Phillips, hex keys, wire strippers, zip ties  
- **Optional-but-recommended**:  
  - Logic probe or multimeter for sanity checks  
  - Spare nozzles and bed tape  

---

## 📋 Bill of Materials

| Item                                      | Qty | Source / Notes                        |
|-------------------------------------------|:--:|---------------------------------------|
| Innocube3D IDEX Klipper Conversion Kit    | 1  | Check kit contents before starting    |
| Raspberry Pi 4 (2 GB+)                    | 1  | Pi OS + Klipper installation          |
| MicroSD cards (16 GB+)                    | 2  | One for Pi, one for Snapmaker board   |
| USB-A ⇄ USB-A cable                      | 1  | USB to control board connection       |
| Zip ties, cable clips                     | —  | Neaten wiring                         |
| Thermal paste / tape (if swapping hotends)| —  | Optional                              |

---

## 📦 Firmware & Software

1. **Klipper** (latest)  
2. **Moonraker** (for API/OctoDash)  
3. **Mainsail/Fluidd** (web UI)  
4. **KlipperScreen** (optional touch screen UI)  

---

## ⚙️ Step 1: Prep Your Pi
1. Flash Raspberry Pi OS to MicroSD.  
2. SSH in, then:
   ```bash
   sudo apt update && sudo apt upgrade -y
   sudo apt install -y python3-pip git virtualenv
Follow the Klipper official install guide to set up Klipper, Moonraker, and your preferred web UI.

## ⚡ Step 2: Flash the Snapmaker Board
Power off and remove the J1S cover.
Insert a dedicated MicroSD (formatted FAT32).
Copy klipper.bin (built for the J1S’s STM32F4 MCU) onto it.
Power on—board automatically flashes and reboots.
Pro tip: Rename your firmware file firmware-20250708.bin to track versions.

## 🔌 Step 3: Wiring & Hardware Swap
Remove the original extruder harness.
Install Innocube3D wiring harness/boards per the kit’s schematic:
Match stepper motor cables (X2, Y2 extruder motors)
Plug in extra thermistor & heater leads
Route fan and endstop wires neatly
Secure everything with zip ties.

## 📝 Step 4: Klipper Configuration
Copy printer-snapmaker-j1s-idex.cfg from this repo to ~/klipper_config/.
Edit the [mcu] and [extruder] sections to match your board’s serial port and pinout.
Review [stepper_x1] vs. [stepper_x2] and [extruder] vs. [extruder1] for IDEX settings.
Upload via your web UI and RESTART Klipper.

## 🖨️ Step 5: First Print
Preheat both hotends via web UI.
Home all axes (verify endstops).
Run CALIBRATE_PRINTHEAD and CALIBRATE_EXTRA_STEPPERS macros.
Slice a simple STL with dual-extruder profile (e.g., Cura IDEX profile).
Hit “Print” and watch two nozzles dance—pure magic.

## 🐞 Troubleshooting
Nozzle not heating: Check thermistor wiring & heater MOSFET in config.
Stepper skipping: Lower acceleration/jerk in [printer] section.
Web UI errors: Inspect Moonraker logs (~/.moonraker/moonraker.log).
Layer misalignment: Re-run IDEX calibration macros carefully.

## 🤝 Contributing
Fork & clone this repo
Create a feature branch (git checkout -b feature/your-idea)
Tweak configs or add scripts/macros
Submit a PR—no gatekeeping, just solid patches.

## 📝 License
This project is released under the MIT License. Do what you want, just don’t sue me if something goes sideways.

TL;DR: Follow the wiring guide, flash Klipper, tweak the config, and hopefully we’ll be blasting dual-extruder prints in no time. Questions, issues, or war stories? Open an Issue and let’s debug together.

— InventedTiME
