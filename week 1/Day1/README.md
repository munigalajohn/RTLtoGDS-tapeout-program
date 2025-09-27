
# 🚀 Day 1: Introduction to Verilog RTL Design & Synthesis

Welcome to **Day 1** of the **RTL Design & Synthesis Workshop**!  
Today, you'll embark on your journey into digital circuit design using:

- **Verilog HDL** — To describe digital logic  
- **Icarus Verilog (iverilog)** — For simulation  
- **Yosys** — For RTL-to-Gate-Level synthesis  

This guide is fully hands-on and designed to build a **strong foundation** in digital hardware design.

---

## 📚 Table of Contents

1. [What is a Simulator, Design, and Testbench?](#1-what-is-a-simulator-design-and-testbench)  
2. [Getting Started with iverilog](#2-getting-started-with-iverilog)  
3. [Lab: Simulating a 2-to-1 Multiplexer](#3-lab-simulating-a-2-to-1-multiplexer)  
4. [Verilog Code Analysis](#4-verilog-code-analysis)  
5. [Introduction to Yosys & Gate Libraries](#5-introduction-to-yosys--gate-libraries)  
6. [Synthesis Lab with Yosys](#6-synthesis-lab-with-yosys)  
7. [Summary](#7-summary)

---

## 1. What is a Simulator, Design, and Testbench?

### 🖥️ Simulator

A **simulator** is a tool that tests your digital circuit by applying inputs and checking outputs — helping you verify correctness before hardware implementation.

### ⚙️ Design

The **design** is your Verilog module that describes the digital logic.

### 🧪 Testbench

A **testbench** provides input stimulus to the design and monitors its output response.

<div align="center">
  <img src="https://github.com/user-attachments/assets/93927b96-df80-4da5-b801-284fc2cc6757" alt="Design & Testbench Overview" width="70%">
</div>

---

## 2. Getting Started with iverilog

**iverilog** is an open-source Verilog simulator. Here's the standard flow:

<div align="center">
  <img src="https://github.com/user-attachments/assets/3ca190fb-cfa4-4abb-b9e1-0151b3c4bdba" alt="iverilog Simulation Flow" width="70%">
</div>

- The design and testbench are passed into `iverilog`.
- The simulator generates a `.vcd` waveform file for GTKWave.

---

## 3. Lab: Simulating a 2-to-1 Multiplexer

### 🔹 Step 1: Clone the Repository

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

### 🔹 Step 2: Install Tools

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

### 🔹 Step 3: Compile & Simulate

```bash
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```

<div align="center">
  <img src="
# 🚀 Day 1: Introduction to Verilog RTL Design & Synthesis

Welcome to **Day 1** of the **RTL Design & Synthesis Workshop**!  
Today, you'll embark on your journey into digital circuit design using:

- **Verilog HDL** — To describe digital logic  
- **Icarus Verilog (iverilog)** — For simulation  
- **Yosys** — For RTL-to-Gate-Level synthesis  

This guide is fully hands-on and designed to build a **strong foundation** in digital hardware design.

---

## 📚 Table of Contents

1. [What is a Simulator, Design, and Testbench?](#1-what-is-a-simulator-design-and-testbench)  
2. [Getting Started with iverilog](#2-getting-started-with-iverilog)  
3. [Lab: Simulating a 2-to-1 Multiplexer](#3-lab-simulating-a-2-to-1-multiplexer)  
4. [Verilog Code Analysis](#4-verilog-code-analysis)  
5. [Introduction to Yosys & Gate Libraries](#5-introduction-to-yosys--gate-libraries)  
6. [Synthesis Lab with Yosys](#6-synthesis-lab-with-yosys)  
7. [Summary](#7-summary)

---

## 1. What is a Simulator, Design, and Testbench?

### 🖥️ Simulator

A **simulator** is a tool that tests your digital circuit by applying inputs and checking outputs — helping you verify correctness before hardware implementation.

### ⚙️ Design

The **design** is your Verilog module that describes the digital logic.

### 🧪 Testbench

A **testbench** provides input stimulus to the design and monitors its output response.

<div align="center">
  <img src="https://github.com/user-attachments/assets/93927b96-df80-4da5-b801-284fc2cc6757" alt="Design & Testbench Overview" width="70%">
</div>

---

## 2. Getting Started with iverilog

**iverilog** is an open-source Verilog simulator. Here's the standard flow:

<div align="center">
  <img src="https://github.com/user-attachments/assets/3ca190fb-cfa4-4abb-b9e1-0151b3c4bdba" alt="iverilog Simulation Flow" width="70%">
</div>

- The design and testbench are passed into `iverilog`.
- The simulator generates a `.vcd` waveform file for GTKWave.

---

## 3. Lab: Simulating a 2-to-1 Multiplexer

### 🔹 Step 1: Clone the Repository

```bash
git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git
cd sky130RTLDesignAndSynthesisWorkshop/verilog_files
```

### 🔹 Step 2: Install Tools

```bash
sudo apt install iverilog
sudo apt install gtkwave
```

### 🔹 Step 3: Compile & Simulate

```bash
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```

<div align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day1/mux%20_lab.png"
</div>

---

## 4. Verilog Code Analysis

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

✔️ If `sel = 1`, output is `i1`  
✔️ If `sel = 0`, output is `i0`

---

## 5. Introduction to Yosys & Gate Libraries

### 🔷 What is Yosys?

**Yosys** converts your Verilog RTL into a **gate-level netlist**.

### 🔧 Why Multiple Gate Flavors in `.lib` Files?

| Property       | Purpose |
|----------------|---------|
| Performance    | Faster switching |
| Power          | Lower energy |
| Area           | Compact size |
| Drive Strength | Handles large loads |
| Noise Immunity | Stable for long wires |

---



## 7. Summary

✅ You learned the difference between **Simulator**, **Design**, and **Testbench**  
✅ You ran a **Verilog simulation** using iverilog & GTKWave  
✅ You analyzed a **2-to-1 Multiplexer**  
✅ You performed **synthesis using Yosys** and explored **gate libraries**

---

🎯 **Next: Proceed to _Day 2 – Combinational Logic & Timing Analysis_**
"
</div>

---

## 4. Verilog Code Analysis

```verilog
module good_mux (input i0, input i1, input sel, output reg y);
always @ (*)
begin
    if(sel)
        y <= i1;
    else 
        y <= i0;
end
endmodule
```

✔️ If `sel = 1`, output is `i1`  
✔️ If `sel = 0`, output is `i0`

---

## 5. Introduction to Yosys & Gate Libraries

### 🔷 What is Yosys?

**Yosys** converts your Verilog RTL into a **gate-level netlist**.

### 🔧 Why Multiple Gate Flavors in `.lib` Files?

| Property       | Purpose |
|----------------|---------|
| Performance    | Faster switching |
| Power          | Lower energy |
| Area           | Compact size |
| Drive Strength | Handles large loads |
| Noise Immunity | Stable for long wires |

---

## 6. Synthesis Lab with Yosys

```bash
yosys
read_liberty -lib /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog /path/to/good_mux.v
synth -top good_mux
abc -liberty /path/to/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

<div align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day1/yosys-intro.png">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day1/yosys-intro.png">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day1/yosys-goodmux_net.png">
</div>

---

## 7. Summary

✅  learned the difference between **Simulator**, **Design**, and **Testbench**  
✅  ran a **Verilog simulation** using iverilog & GTKWave  
✅  analyzed a **2-to-1 Multiplexer**  
✅  performed **synthesis using Yosys** and explored **gate libraries**

---

🎯 **Next: Proceed to _Day 2 – Combinational Logic & Timing Analysis_**

