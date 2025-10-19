

# Day 4 – Noise Margin of CMOS Inverter

## 1. What is Noise Margin?

In a CMOS inverter, the **Voltage Transfer Characteristic (VTC)** describes how the output voltage (Vout) changes with input voltage (Vin). From the VTC curve, we define four important voltage levels:

| Parameter | Meaning |
|-----------|---------|
| **VOH** (Output High Voltage) | Maximum output voltage (logic ‘1’) when input is LOW |
| **VOL** (Output Low Voltage) | Minimum output voltage (logic ‘0’) when input is HIGH |
| **VIH** (Input High Voltage)  | Minimum input voltage recognized as logic ‘1’ |
| **VIL** (Input Low Voltage)   | Maximum input voltage recognized as logic ‘0’ |

On the **VTC curve**:
- VOH appears at the top flat region where the output is approximately VDD.
- VOL appears at the bottom flat region where the output is approximately 0V.
- VIL and VIH occur where the **slope of the VTC equals –1** (maximum gain points).

These two points define the **transition region** of the inverter.

---

## 2. How to Calculate Noise Margins

A digital circuit must tolerate some noise without causing logic errors.  
Noise margin tells us **how much noise can be added** without flipping the logic level.

There are two types of noise margin:

### High-level noise margin (NMH)

Amount of noise the **HIGH output** can tolerate:

**NMH = VOH – VIH**

### Low-level noise margin (NML)

Amount of noise the **LOW output** can tolerate:

**NML = VIL – VOL**

---

## 3. Why Noise Margin Matters

- Ensures reliable logic operation
- Protects against noise, voltage drops, coupling, crosstalk
- Larger noise margins = more robust inverter
- Good inverter design tries to make **NMH ≈ NML**

---

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%204/day4_wave.png" 
       alt="day4 terminal" width="600"/>
</p>

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%204/Screenshot%202025-10-18%20210314.png" 
       alt="day4 terminal" width="600"/>
</p>

### From the Graph:

Low noise margin = VIL – VOL = 0.766-0.130

Low noise margin = 0.636


High noise margin = VOH – VIH = 1.732-0.969

High noise margin = 0.754


<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%204/day4_different_values.png" 
       alt="day4 noise margin" width="600"/>
</p>

