# Day 4: Gate-Level Simulation (GLS), Blocking vs. Non-Blocking in Verilog, and Synthesis-Simulation Mismatch

Welcome to Day 4 of the RTL Workshop! Today’s session focuses on three essential topics in digital design:

- **Gate-Level Simulation (GLS)**
- **Blocking vs. Non-Blocking Assignments in Verilog**
- **Synthesis-Simulation Mismatch**

---

## Table of Contents

- [1. Gate-Level Simulation (GLS)](#1-gate-level-simulation-gls)
- [2. Synthesis-Simulation Mismatch](#2-synthesis-simulation-mismatch)
- [3. Blocking vs. Non-Blocking Assignments in Verilog](#3-blocking-vs-non-blocking-assignments-in-verilog)
  - [3.1 Blocking Statements](#31-blocking-statements)
  - [3.2 Non-Blocking Statements](#32-non-blocking-statements)
  - [3.3 Comparison Table](#33-comparison-table)
- [4. Labs](#4-labs)
- [5. Summary](#5-summary)

---

## 1. Gate-Level Simulation (GLS)

**GLS** stands for **Gate-Level Simulation** — used to validate:

✅ Functional correctness  
✅ Timing (with SDF delays)  
✅ Power and scan chain verification

**When?**  
✔️ After synthesis, ✔️ Before physical design

**Types of GLS:**
| Type          | Timing          |
|---------------|-----------------|
| Functional GLS | Zero/Unit Delay |
| Timing GLS    | With SDF        |

---

## 2. Synthesis-Simulation Mismatch

Occurs when **pre-synthesis simulation ≠ post-synthesis behavior**.

**Common Causes:**
- Non-synthesizable code (`#delay`, `initial`, etc.)
- Incomplete sensitivity lists
- Wrong use of `=` and `<=`

💡 **Always write RTL with synthesizable constructs only.**

---

## 3. Blocking vs. Non-Blocking Assignments in Verilog

### 3.1 Blocking (`=`)

```verilog
always @(*) y = a & b;
```

✅ Sequential execution  
✅ Used for **combinational logic**

---

### 3.2 Non-Blocking (`<=`)

```verilog
always @(posedge clk) q <= d;
```

✅ Parallel update  
✅ Used for **sequential logic (flip-flops)**

---

### 3.3 Comparison Table

| Feature                  | Blocking (`=`)             | Non-Blocking (`<=`)         |
|--------------------------|----------------------------|-----------------------------|
| Execution                | Immediate (Sequential)     | Scheduled (Parallel)        |
| Suitable For             | Combinational Logic        | Sequential Logic            |
| Ordering Sensitive?      | Yes                        | No                          |
| Typical Use Case         | `always @(*)`              | `always @(posedge clk)`     |

---

## 4. Labs

---

### Lab 1: Ternary Operator MUX

```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

![lab1](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day4/iverilog-ternary_operator.png)

---

### Lab 2: Synthesis Using Yosys

![lab2](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day4/yosys-ternary.png)

---

### Lab 3: GLS of MUX

```shell
iverilog /path/to/primitives.v /path/to/sky130_fd_sc_hd.v ternary_operator_mux.v testbench.v
```

![lab3](https://github.com/Shaikhaseena16/RISC-V_VSDIAT/blob/main/week%201/Day4/gls-ternary_operator.png)

---

### Lab 4: Bad MUX Example

```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
  always @ (sel) begin
    if (sel)
      y <= i1;
    else 
      y <= i0;
  end
endmodule
```

✅ **Problem**: Wrong sensitivity list & wrong assignment style.

---

### Corrected Version

```verilog
always @ (*) begin
  if (sel)
    y = i1;
  else
    y = i0;
end
```

![lab4](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day4/iverilog-bad.png)

---

### Lab 5: GLS of Bad MUX

![lab5](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day4/gls-bad_mux.png)

---

### Lab 6: Blocking Caveat

```verilog
module blocking_caveat (input a, input b, input c, output reg d);
  reg x;
  always @ (*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```

✅ Fix ordering:

```verilog
always @ (*) begin
  x = a | b;
  d = x & c;
end
```

![lab6](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day4/iverilog-blocking.png)

---

### Lab 7: Synthesis of Fixed Module

![lab7](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day4/yosys-blocking.png)

---

## 5. Summary

| Topic                     | Key Takeaway |
|--------------------------|--------------|
| Gate-Level Simulation     | Validates synthesized netlist |
| Synthesis-Simulation Mismatch | Avoid with clean and synthesizable RTL |
| Blocking (`=`)           | Use for combinational |
| Non-Blocking (`<=`)      | Use for sequential |

---

> ✅ **Golden Rule:**  
> **`=` for Combinational Logic** — **`<=` for Sequential Logic**

---

Use = for combinational logic, <= for sequential logic.

@(*) prevents latch inference in combinational blocks.

Missing else/default causes unintended latches.

Never mix blocking and non-blocking in the same always block.
