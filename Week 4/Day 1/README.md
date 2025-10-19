
# Day 1 – NMOS Drain Current (Id) vs Drain-to-Source Voltage (Vds) using SPICE (Sky130)

This document provides a complete theoretical and practical understanding of how NMOS drain current (Id) varies with drain-to-source voltage (Vds), including:
- Device physics
- Threshold voltage & body effect
- Linear & saturation operation regions
- Id-Vds equation derivation
- Channel length modulation (λ)
- SPICE setup and simulation using Sky130 PDK
- Plot interpretation and key learnings

---

## 📚 Table of Contents

1. [Introduction to Circuit Design and SPICE Simulations](#1-introduction-to-circuit-design-and-spice-simulations)  
2. [NMOS Structure & 4-Terminal Device](#2-nmos-structure--4-terminal-device)  
3. [Threshold Voltage (Vt) & Body Effect](#3-threshold-voltage-vt--body-effect)  
4. [NMOS Resistive (Linear) & Saturation Regions of Operation](#4-nmos-resistive-linear--saturation-regions-of-operation)  
5. [First-Order Derivation of Id vs Vds](#5-first-order-derivation-of-id-vs-vds)  
6. [Channel Length Modulation (λ)](#6-channel-length-modulation-λ)  
7. [Lab Screenshots & Graphs](#7-lab-screenshots--graphs)

---

## 1. Introduction to Circuit Design and SPICE Simulations

In VLSI design, MOSFETs (NMOS/PMOS) are the fundamental building blocks of digital and analog circuits.  
Before fabrication, circuit behavior must be analyzed using **SPICE simulations** to ensure correct operation, performance, and reliability.

### ✅ Why simulate?
- Avoid fabrication errors
- Predict current/voltage behavior
- Optimize circuits before layout
- Save time and cost

### ✅ What is SPICE?
**SPICE (Simulation Program with Integrated Circuit Emphasis)** is a powerful circuit simulator that models real device physics and predicts circuit behavior under different bias conditions.

### ✅ Basic SPICE workflow:
1. Write circuit netlist
2. Include transistor model (.lib/.model)
3. Apply bias voltages/currents
4. Run simulation (.op, .dc, .tran, etc.)
5. Plot results and analyze

---

## 2. NMOS Structure & 4-Terminal Device

An NMOS is a **4-terminal, voltage-controlled device** built on a **p-type substrate** with two **n+ diffusion regions** forming Source and Drain. A **thin SiO₂ oxide** separates the Gate from the substrate.

### ✅ Terminals:
- **Gate (G)** – Controls channel formation
- **Source (S)** – Reference terminal (usually 0V)
- **Drain (D)** – Carries current (Id)
- **Body/Bulk (B)** – P-substrate (usually tied to Source to avoid body effect)


### ✅ Default biasing:
- Source = 0V
- Body = Source (to avoid Vsb)
- Gate = positive voltage (Vgs)
- Drain = positive voltage (Vds)

### ✅ Current direction:
- Electrons flow: Source → Drain
- Conventional current Id: Drain → Source

---
## 3. Threshold Voltage (Vt) & Body Effect

The **Threshold Voltage (Vt)** is the minimum gate-to-source voltage (Vgs) required to create a **strong inversion channel** between the source and drain, allowing current (Id) to flow.

---

### 3.1 Channel Formation Stages

| Condition     | Channel Formed? | MOSFET State |
|---------------|-----------------|----------------|
| Vgs = 0        | No              | OFF |
| Vgs < Vt       | Weak inversion  | OFF |
| Vgs = Vt       | Just inversion  | Start of conduction |
| Vgs > Vt       | Strong inversion| ON (conducting) |

---

### 3.2 Physical Meaning of Vt

As Vgs increases:
- Positive gate voltage repels holes from the p-substrate.
- A **depletion region** forms (fixed negative ions).
- At **Vgs = Vt**, sufficient electrons accumulate at the surface,
  creating an **n-type inversion layer (channel)**.
- Current can now flow from Drain to Source when Vds > 0.

✅ Therefore, **Vt controls ON/OFF behavior of NMOS.**

---

### 3.3 Body Effect (Substrate Bias Effect)

Normally:  
- Source (S) = 0V  
- Body (B) = Source  
⇒ `Vsb = 0` → No body effect

If **Body is at a lower potential than Source (Vsb > 0)**:
- Depletion region widens.
- More gate voltage needed to invert the surface.
- **Vt increases.**

✅ This is known as the **Body Effect.**

---

### 3.4 Threshold Voltage with Body Effect

`Vt = Vto + γ( √(2φf + Vsb) - √(2φf) )`

Where:
- `Vto` = threshold voltage at zero body bias (`Vsb = 0`)
- `γ` = body effect coefficient
- `φf` = Fermi potential
- `Vsb` = Source-to-body voltage

✅ If `Vsb = 0` → `Vt = Vto`  
✅ If `Vsb > 0` → `Vt > Vto`

---

### 3.5 Key Points
- Vt is **not constant** → depends on body bias.
- Body effect **increases Vt** → transistor harder to turn ON.
- Designers **tie Body to Source** to avoid Vt shift.
- SPICE models automatically include body effect.

---

## 4. NMOS Resistive (Linear) & Saturation Regions of Operation

Once **Vgs > Vt**, a conductive **n-type channel** forms between Source and Drain.  
Now, the behavior of the MOSFET depends on **Drain-to-Source voltage (Vds)**.

The MOSFET operates in **two major regions**:

---

### 4.1 Linear / Ohmic / Triode / Resistive Region

**Condition:**  
`Vds < (Vgs - Vt)`

In this region:
- Channel exists across the entire length.
- Channel depth is almost uniform.
- MOSFET behaves like a **voltage-controlled resistor**.
- Drain current increases **linearly with Vds** (for small Vds).

✅ First-order drain current equation:
`Id = kn * [ (Vgs - Vt) * Vds - (Vds^2 / 2) ]`

Where:
- `kn = μn * Cox * (W/L)` (transconductance parameter)

If Vds is very small:
`Id ≈ kn * (Vgs - Vt) * Vds` → Ohm’s Law behavior

✅ Therefore, this region is called **resistive or linear**.

---

### 4.2 Saturation (Active) Region

**Condition:**  
`Vds ≥ (Vgs - Vt)`

In this region:
- Channel “pinches off” near the drain.
- Further increase in Vds **does not significantly increase Id**.
- MOSFET behaves like a **current source**.

✅ Ideal saturation current:
`Id = (kn / 2) * (Vgs - Vt)^2`

✅ Id becomes (ideally) constant.

---

### 4.3 Pinch-Off Concept (Important for understanding saturation)

- Voltage along channel increases from Source (0V) to Drain (≈ Vds).
- At the drain end, effective gate voltage is `Vgs - Vds`.
- When `Vgs - Vds = Vt`, channel disappears at the drain → **pinch-off** point.

Even though channel pinches off, electrons still flow through a narrow high-field region into the drain.

✅ Therefore, **current continues (saturates), not stops.**

---

### 4.4 Region Summary Table

| Region   | Condition | Behavior         |
|----------|-----------|------------------|
| **Linear** | `Vds < (Vgs - Vt)` | Acts like **resistor** (Id ∝ Vds) |
| **Saturation** | `Vds ≥ (Vgs - Vt)` | Acts like **current source** (Id ≈ constant) |

---

## 5. First-Order Derivation of Id vs Vds

We derive the MOSFET drain current (Id) using:
- Charge in the channel
- Carrier velocity (drift current)
- Integration from Source to Drain

---

### 5.1 Inversion Charge at any point x in channel

At position `x` along the channel:
- Local channel voltage = `V(x)`
- Effective gate-to-channel voltage = `Vgs - V(x)`
- Effective voltage above threshold = `Vgs - V(x) - Vt`

✅ Inversion charge per unit area:
`Qi(x) = -Cox * (Vgs - V(x) - Vt)`

(Negative sign because electrons = negative charge)

---

### 5.2 Drain Current = Charge × Velocity × Width

Electron drift velocity:
`Vn(x) = μn * E = μn * (dV/dx)`

Drain current:
`Id = Qi(x) * Vn(x) * W`

Substitute:
`Id = [-Cox * (Vgs - V(x) - Vt)] * [μn * (dV/dx)] * W`

Rearrange:
`Id = μn * Cox * W * (Vgs - V(x) - Vt) * (dV/dx)`

---

### 5.3 Separate variables & integrate from Source to Drain

Left side:
`∫ Id dx = Id * L`

Right side:
`∫ μn * Cox * W * (Vgs - V - Vt) dV` from 0 to Vds

So:
`Id * L = μn * Cox * W * [ (Vgs - Vt) * Vds - (Vds^2 / 2) ]`

Divide both sides by L:

`Id = μn * Cox * (W/L) * [ (Vgs - Vt) * Vds - (Vds^2 / 2) ]`

Define:
`kn = μn * Cox * (W/L)`

✅ Final Linear Region Equation:
`Id = kn * [ (Vgs - Vt) * Vds - (Vds^2 / 2) ]`

---

### 5.4 Saturation Condition

At the edge of saturation:
`Vds = Vgs - Vt`

Substitute into linear equation:

`Id(sat) = kn * [ (Vgs - Vt)^2 - ( (Vgs - Vt)^2 / 2 ) ]`
`Id(sat) = (kn / 2) * (Vgs - Vt)^2`

✅ Ideal Saturation Equation:
`Id = (kn / 2) * (Vgs - Vt)^2`

---

### 5.5 Summary

| Region | Id Equation |
|--------|-------------|
| **Linear** | `Id = kn * [ (Vgs - Vt) * Vds - (Vds^2 / 2) ]` |
| **Saturation** | `Id = (kn / 2) * (Vgs - Vt)^2` |

✅ Note: This is **first-order model (no channel length modulation).**

---
## 6. Channel Length Modulation (λ)

In ideal saturation, Id is constant.  
**But in real MOSFETs, Id slightly increases with Vds.**

Why?
- As Vds increases, the **depletion region at the drain widens**
- This **reduces the effective channel length**
- Shorter channel → **more current**
- Similar to **Early effect in BJTs**

✅ This effect is called **Channel Length Modulation (CLM)**  
✅ Modeled using **λ (lambda)**

### Saturation Current with λ:
`Id = (kn / 2) * (Vgs - Vt)^2 * (1 + λ * Vds)`

- If `λ = 0` → ideal (flat curve)
- If `λ > 0` → realistic (slight slope)

✅ Therefore: MOSFET in saturation ≠ perfect current source  
→ It has **finite output resistance (ro)**

---

## 7. Lab Screenshots & Graphs

To simulate NMOS behavior realistically, SPICE uses **foundry-provided transistor models**.

### ✅ What the model file contains:

- Mobility (μn)
- Oxide capacitance (Cox)
- Body effect parameters (γ, φf)
- Channel length modulation (λ)
- Short-channel effects
- Parasitics, leakage, etc.

## steps for ngspice

git clone https://github.com/kunalg123/sky130CircuitDesignWorkshop/

cd sky130CircuitDesignWorkshop/design/

vim day1_nfet_idvds_L2_W5.spice

ngspice day1_nfet_idvds_L2_W5.spice

plot -vdd#branch

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%201/day1_vds.png" 
       alt="cloning" width="600"/>
</p>



<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%204/Day%201/day1_zoomed.png" 
       alt="day1" width="600"/>
</p>


