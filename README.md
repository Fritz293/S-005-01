# Raspberry Pi Multi-Status LED HAT

An industrial-grade Raspberry Pi HAT designed for high-visibility status monitoring using the **OSRAM DISPLIX® P3333 RGB LED** array. This board features professional-level power protection and follows the official Raspberry Pi HAT specifications.

## 🚀 Current Progress: Phase 1 (Schematic Complete)
- [x] Power Filtering & ESD Protection
- [x] High-Efficiency Reverse Polarity Protection (P-MOSFET)
- [x] Logic Level Shifting for 5V LED Driving
- [x] HAT ID EEPROM Implementation
- [ ] PCB Layout & Routing (Next Step)

---

## 🛠 Hardware Specifications

### 1. Power & Protection
To ensure the Raspberry Pi 5 remains stable during transient spikes, the 5V rail includes:
* **Reverse Polarity Protection:** Implemented via an **AO3401A P-Channel MOSFET**. This provides protection with negligible voltage drop (<0.05V) compared to a standard Schottky diode.
* **Overvoltage Protection:** Dual **5.6V 3W Zener/TVS diodes** ($D10, D11$).
* **Current Limiting:** A **375mA Fuse (F1)** protects the logic from downstream shorts.
* **Filtering:** 10µF and 100nF ceramic/tantalum capacitors for noise decoupling.

### 2. LED Driver Circuit
The board drives an **OSRAM KRTB LSLPS1.32** RGB LED via a high-side/low-side configuration:
* **Switching:** Three **BSS138 N-MOSFETs** act as logic-level shifters, allowing the Pi's 3.3V GPIOs to safely switch the 5V LED rail.
* **Current Calibration (at 20mA):**
    * **Red:** 150Ω
    * **Green:** 120Ω (Updated for Osram Vf)
    * **Blue:** 110Ω (Updated for Osram Vf)
* **ESD Safety:** **PESD5V0S1UL** protection diodes are placed at the external connector and MOSFET drains to prevent static damage.

### 3. Controller Interface
| Function | GPIO | Physical Pin |
| :--- | :--- | :--- |
| **Power/Idle** | GPIO 17 | Pin 11 |
| **Media Activity** | GPIO 27 | Pin 13 |
| **Error State** | GPIO 22 | Pin 15 |
| **ID_SDA** | EEPROM Data | Pin 27 |
| **ID_SCL** | EEPROM Clock | Pin 28 |

---


## 📝 Design Notes
> **Note on EEPROM Wiring:** The AT24C32 EEPROM is connected to the dedicated `ID_SD` and `ID_SC` pins (Pins 27/28) to allow the Raspberry Pi to automatically load the device tree overlay at boot.