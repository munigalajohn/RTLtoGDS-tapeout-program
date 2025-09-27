# Day 3: Combinational and Sequential Optimization

Welcome to Day 3 of this workshop! Today we discuss optimization of combinational and sequential circuits, introducing techniques to enhance efficiency and performance.

---

## Table of Contents

- [1. Constant Propagation](#1-constant-propagation)
- [2. State Optimization](#2-state-optimization)
- [3. Cloning](#3-cloning)
- [4. Retiming](#4-retiming)
- [5. Labs on Optimization](#5-labs-on-optimization)
  - [Lab 1](#lab-1)
  - [Lab 2](#lab-2)
  - [Lab 3](#lab-3)
  - [Lab 4](#lab-4)
  - [Lab 5](#lab-5)
  - [Lab 6](#lab-6)

---

## 1. Constant Propagation

Constant propagation is a compiler optimization technique used to simplify logic by replacing variables with constant values during synthesis.

**Benefits:**
- ✅ Reduced Circuit Complexity  
- ✅ Faster Performance  
- ✅ Lower Power and Area  

![Constant Propagation Example](https://github.com/user-attachments/assets/d7f06056-66c1-44af-99a8-623fdf5879be)

---

## 2. State Optimization

State Optimization improves FSM efficiency by reducing redundant states and optimizing state encoding.

**Methods:**
- State Reduction  
- State Encoding Optimization  
- Logic Minimization  
- Power Optimization (e.g., Clock Gating)

---

## 3. Cloning

Cloning duplicates logic to **improve timing and reduce loading** on critical paths.

![Cloning Example](https://github.com/user-attachments/assets/6bdd2c12-02a2-4ea5-895c-98e349b93bac)

---

## 4. Retiming

Retiming repositions **flip-flops/registers** in the circuit without changing functionality to **balance delays**.

---

## 5. Labs on Optimization

---

### Lab 1

```verilog
module opt_check (input a , input b , output y);
	assign y = a?b:0;
endmodule
```

Run synthesis with:

```shell
opt_clean -purge
```

![Lab 1 Output](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day3/yosys-opt_check.png)

---

### Lab 2

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

![Lab 2 Output](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day3/yosys-opt_check2.png)

---

### Lab 3

```verilog
module opt_check2 (input a , input b , output y);
	assign y = a?1:b;
endmodule
```

![Lab 3 Output](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day3/yosys-opt_check3.png)

---

### Lab 4

```verilog
module opt_check4 (input a , input b , input c , output y);
 assign y = a?(b?(a & c ):c):(!c);
endmodule
```

✅ Simplifies to:

```verilog
assign y = a ? c : !c;
```

![Lab 4 Output](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day3/yosys-opt_check4.png)

---

### Lab 5

```verilog
module dff_const1(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b0;
	else
		q <= 1'b1;
end
endmodule
```

![Lab 5 Output](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day3/yosys-asyncset.png)

---

### Lab 6

```verilog
module dff_const2(input clk, input reset, output reg q);
always @(posedge clk, posedge reset)
begin
	if(reset)
		q <= 1'b1;
	else
		q <= 1'b1;
end
endmodule
```

![Lab 6 Output](https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/week%201/Day3/yosys-dff-const2.png)

---

## ✅ Summary

| Technique           | Purpose                          |
|--------------------|----------------------------------|
| Constant Propagation | Remove redundant logic          |
| State Optimization   | Simplify FSMs                  |
| Cloning             | Improve timing & reduce load     |
| Retiming            | Balance delays using registers   |

---

Learned combinational vs sequential logic in Verilog.

Procedural blocks (always) used for writing logic behavior.

Case and If-Else statements help define clean decision-making logic.

Order of execution matters, especially for synthesizable RTL.
