
# Day 2 – Velocity Saturation and Basics of CMOS Inverter VTC

This document provides a complete theoretical and practical understanding of how **short-channel effects (velocity saturation)** influence MOSFET drain current and how these effects impact the **CMOS inverter and its Voltage Transfer Characteristic (VTC)**.

Topics covered include:
- Long-channel vs short-channel MOSFETs  
- Velocity saturation concept and physical meaning  
- Impact of VSAT on Id–Vds curve  
- Unified MOSFET current model  
- CMOS inverter operation basics  
- Load-line method to derive inverter VTC  
- SPICE simulations using Sky130 PDK

---

## 1. Introduction to Velocity Saturation & CMOS Inverter

In Day 1, we studied ideal MOSFET behavior assuming long-channel devices and derived the Id–Vds equations using the square-law model.

However, in modern technologies like Sky130 (130 nm), MOSFETs are short-channel devices and do **not** follow the ideal square-law behavior.

Before designing CMOS logic circuits, it is important to understand:
- Why real MOSFETs behave differently
- Why current saturates earlier than expected
- How this impacts CMOS inverter switching
- How to derive the voltage transfer characteristic (VTC)

Velocity saturation is one of the most important short-channel effects that shapes the real Id–Vds characteristics.

---

## 2. Long-Channel vs Short-Channel MOSFET Behavior

| Parameter | Long-Channel (Ideal) | Short-Channel (Real, Sky130) |
|-----------|-----------------------|-------------------------------|
| Channel length | Large | Small |
| Velocity vs Electric Field | Linear (v = μE) | Saturates (v → v_sat) |
| Id–Vds curve | Follows square law | Flattens early |
| Saturation onset | Later | Earlier |
| Peak current | Higher | Lower |
| Modeling | Simple equations | Advanced models (BSIM) |

In modern deep submicron technologies, short-channel effects dominate, and we must account for velocity saturation to understand real MOSFET behavior.

---

## 3. Velocity Saturation (VSAT) & Physical Meaning

### 3.1 What is Velocity Saturation?

In ideal MOSFET theory, electron velocity increases linearly with electric field:

v = μn × E

But in real short-channel devices, when the electric field becomes very high, electrons cannot accelerate indefinitely. They reach a maximum velocity known as **saturation velocity (v_sat).**

After this point:
- Increasing Vds further does **not** increase electron velocity.
- Drain current stops increasing linearly.

This phenomenon is called **velocity saturation (VSAT).**

---

### 3.2 Why does it occur?

As Vds increases:
- Electric field near the drain becomes very high.
- Electrons gain kinetic energy and collide with the silicon lattice.
- Scattering increases.
- Mobility decreases.
- Velocity reaches a constant maximum (v_sat).

Therefore, even though Vds increases, carrier velocity stops increasing, causing current saturation earlier than predicted by the ideal square-law model.

---

### 3.3 Key Consequences

- Id–Vds curve becomes flatter at high Vds.
- Current saturates earlier (at lower Vds).
- Peak current is reduced.
- MOSFET no longer follows the (Vgs – Vt)² behavior exactly.

---

## 4. Impact of Velocity Saturation on Drain Current (Id–Vds)

### 4.1 Ideal MOSFET saturation (from Day 1)

In ideal conditions (no velocity saturation), saturation current is:

Id(sat) = (kn / 2) × (Vgs – Vt)²

This assumes:
- Constant mobility (μn)
- No velocity limit
- No channel length modulation

### 4.2 Real MOSFET with velocity saturation

In real short-channel devices:

Id = [ μn × Cox / (1 + (Vds / (Ec × L)) ) ] × (W/L) × [ (Vgs – Vt) × Vds – (Vds² / 2) ]

Where:
- Ec = critical electric field at which velocity starts to saturate
- L = channel length

The denominator term reduces current when Vds is high, causing early current saturation.

### 4.3 Comparison (from slides)

| Behavior | Long-Channel | Short-Channel |
|----------|--------------|----------------|
| Follows square-law | Yes | No |
| Saturation onset | Later | Earlier |
| Peak current | Higher | Lower |
| Id–Vds curve shape | Steeper | Flatter |

✅ This explains why Sky130 (short-channel) MOSFETs show early saturation and lower current.

---

## 5. Unified MOSFET Current Model (All Regions)

Instead of using separate equations for linear and saturation regions, modern SPICE models use a **unified current model** that automatically transitions between:

- Linear region
- Velocity saturation region
- Saturation region

---
## 6. CMOS Inverter Structure & Operation

A CMOS inverter consists of:
- PMOS transistor connected to VDD (pull-up network)
- NMOS transistor connected to GND (pull-down network)
- Output node connected between PMOS and NMOS
- Input connected to both gates

### When Vin = 0:
- NMOS: OFF (Vgs = 0)
- PMOS: ON (Vsg = VDD > |Vtp|)
- Output is pulled up to VDD (logic 1)

### When Vin = VDD:
- NMOS: ON (Vgs = VDD > Vtn)
- PMOS: OFF (Vsg = 0)
- Output is pulled down to 0V (logic 0)

✅ Therefore, CMOS inverter produces the logical NOT of the input.

✅ Ideal behavior: Full logic swing from 0 to VDD.

✅ Very low static power consumption (only one transistor ON at a time).

---

## 7. NMOS & PMOS Switching Behavior in Inverter

To understand the transfer characteristic (VTC), we analyze how each transistor behaves as Vin changes gradually from 0 to VDD.

### 7.1 Important voltage relationships:
For NMOS:
- Vgs_n = Vin
- Vds_n = Vout

For PMOS:
- Vsg_p = VDD – Vin
- Vsd_p = VDD – Vout

### 7.2 As Vin increases:
1. For small Vin:
   - NMOS is OFF
   - PMOS is ON
   - Vout ≈ VDD

2. As Vin reaches threshold:
   - NMOS begins to conduct
   - PMOS still partially ON
   - Both transistors conduct → output starts falling

3. At Vin ≈ switching threshold (Vm):
   - NMOS and PMOS currents are equal
   - Output rapidly transitions

4. At large Vin:
   - NMOS fully ON
   - PMOS OFF
   - Vout ≈ 0

✅ The point where NMOS current = PMOS current determines the voltage transfer curve.

---

## 8. Load Line Method & CMOS Inverter Transfer Characteristic (VTC)

The VTC (Voltage Transfer Characteristic) is a plot of Vout vs Vin.

To derive this graph:
1. For a given Vin, write ID_NMOS as a function of Vout.
2. For the same Vin, write ID_PMOS as a function of Vout.
3. Find Vout where ID_NMOS = ID_PMOS.
4. Repeat for different Vin values.
5. Plot (Vin, Vout) points to build the VTC curve.

### Expected shape:
- Starts at Vout = VDD (when Vin = 0)
- Drops slowly at first
- Sharp transition at switching threshold (high gain region)
- Ends at Vout = 0 (when Vin = VDD)

✅ Balanced inverter: switching threshold ≈ VDD/2  
✅ PMOS is usually sized larger due to lower hole mobility.

---

## 9. SPICE Setup & Model Parameters (Sky130)

To simulate realistic MOSFET behavior (including velocity saturation), we must include the Sky130 PDK models.



---

## 10. SPICE Netlist Explanation (Id–Vgs & Id–Vds)

We perform two main simulations: Id–Vgs & Id–Vds

### 10.1 Id–Vgs (to extract threshold voltage Vt)
- Vds is set to a constant value (e.g., 1.8V)
- Vin (Vgs) is swept from 0 to 1.8V
- Plot Id vs Vgs
- Vt ≈ the point where Id starts to rise significantly

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%202/day2%20id_vgs%20graph%20(1).png" 
       alt="day2 id_vgs terminal" width="600"/>
</p>


### 10.2 Id–Vds (to observe velocity saturation)
- Vin (Vgs) is set to multiple values (e.g., 0.8V, 1.0V, 1.2V, 1.4V, 1.6V, 1.8V)
- Vds is swept from 0 to 1.8V
- Plot Id vs Vds for each Vgs
- Observe early saturation and flattening due to velocity saturation

  <p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%202/day2_vds.png" 
       alt="day2" width="600"/>
</p>

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%202/day2_vds_values.png" 
       alt="day2 id_vgs terminal" width="600"/>
</p>

### Threshold voltage
<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%202/day2_vgs.png" 
       alt="day 2 vt" width="600"/>
</p>

✅ These simulations confirm the theory of short-channel behavior.

