



# Day 3 – CMOS Inverter Switching Threshold (Vm) & Dynamic Behavior

This document explains how the **switching threshold (Vm)** of a CMOS inverter is determined and how **transistor sizing affects delay and robustness**. This is essential for designing stable logic gates and high-performance clock buffers.

---

## 1. What is Switching Threshold (Vm)?

The **switching threshold (Vm)** is the input voltage at which:
Vin = Vout
At this point, both transistors are partially ON and the inverter is most sensitive to noise. Vm defines the logic switching point of the inverter.

---

## 2. Current Condition at Vm

At Vm, the currents of PMOS and NMOS must balance:
IdsP = – IdsN or IdsP + IdsN = 0

Both NMOS and PMOS operate in saturation or linear region depending on sizing and voltages.

---

## 3. Current Equations (Conceptual Form)

Using a simplified current model:

IdsN = kn * [(Vm – Vt) * Vdsatn – (Vdsatn² / 2)]

IdsP = kp * [(Vm – Vdd – Vt) * Vdsatp – (Vdsatp² / 2)]

Equate |IdsN| = |IdsP| and solve for Vm.

We focus on the final simplified form rather than full derivation.

---

## 4. Final Simplified Formula for Vm

The switching threshold can be expressed as:

Vm = (R · Vdd) / (1 + R)

where, R = (kp · Vdsatp) / (kn · Vdsatn) = (Wp/Lp) / (Wn/Ln) × (μp / μn)


✅ Therefore, **transistor sizing directly controls Vm.**

---

## 5. Effect of Sizing on Vm

| PMOS Size (Wp/Lp) | NMOS Size (Wn/Ln) | Vm Position     |
|-------------------|-------------------|-----------------|
| Small PMOS        | Equal NMOS        | Vm < Vdd/2      |
| Balanced          | Balanced          | Vm ≈ Vdd/2 ✅    |
| Large PMOS        | Smaller NMOS      | Vm > Vdd/2      |

✅ Proper sizing gives a **symmetrical inverter.**

---

## 6. Rise vs Fall Delay (Dynamic Behavior)

- PMOS charges output → **Rise delay (trise)**
- NMOS discharges output → **Fall delay (tfall)**

If PMOS is **weaker** → rise > fall  
If PMOS is **stronger** → rise < fall

✅ Goal: **Equal rise and fall delay** → Balanced inverter

---

## 7. Example Trend (from simulation)

| Wp/Lp | x·Wn/Ln | Rise Delay | Fall Delay | Vm    |
|-------|---------|------------|------------|-------|
| 1x    | 1x      | 148 ps     | 71 ps      | 0.99V |
| 1x    | 2x      | 80 ps      | 76 ps      | 1.20V ✅ Best balance |
| 1x    | 3x      | 57 ps      | 80 ps      | 1.25V |
| 1x    | 4x      | 45 ps      | 84 ps      | 1.35V |
| 1x    | 5x      | 37 ps      | 88 ps      | 1.40V |

✅ Best inverter occurs when **rise ≈ fall** (2x NMOS in this case).

---

## 8. Why Balanced Vm and Delay Are Important?

Balanced CMOS inverters are used in:
- Clock buffers
- Clock tree synthesis
- Distribution networks

To maintain:
- Consistent pulse width
- Stable duty cycle
- Minimum skew
- High noise immunity

✅ Robust digital design depends on **proper Vm and sizing.**

---

## 9. Key Takeaways

- **Vm occurs when IdsN = IdsP**
- **Vm is controlled by sizing ratio (Wp/Lp vs Wn/Ln)**
- **Vm = (R · Vdd) / (1 + R)**
- **Balanced sizing gives Vm ≈ Vdd/2**
- **Equal rise and fall delay improves performance**
- **Used in high-speed and clock circuits**

---

## 10. Labs

### CMOS inverter
<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%203/day3_vts_wave.png" 
       alt="day3 terminal" width="600"/>
</p>

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%203/day3_vtc_values.png" 
       alt="day3 cmos inv" width="600"/>
</p>


### Transient analysis

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%203/day3_trans_wave.png" 
       alt="day3 trans terminal" width="600"/>
</p>

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%203/day3_trans_values.png" 
       alt="day3 trans" width="600"/>
</p>


### Rise time delay

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%203/day3%20rise.png" 
       alt="day3 rise delay" width="600"/>
</p>

Rise time delay = 1.332

### Fall time delay

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%203/day3%20fall.png" 
       alt="day3 fall delay" width="600"/>
</p>

Fall time delay = 0.285

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%203/day3_different_values.png" 
       alt="day3 fall delay" width="600"/>
</p>

### Switching threshold

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%203/day%203%20switching.png" 
       alt="day3 switching threshold" width="600"/>
</p>
