# VSD task 1 GLS 

## Purpose of Gate-Level Simulation (GLS)
Gate-Level Simulation (GLS) is performed after synthesis to verify the functionality and timing of a design at the gate-level.  
Unlike RTL (Register Transfer Level) simulations, which operate at a behavioral abstraction, GLS uses the netlist generated post-synthesis — containing the actual standard cells and interconnections used in the implementation.

---

## Key Objectives of GLS for BabySoC

### 🧩 1. Timing Verification
- GLS includes Standard Delay Format (SDF) files to incorporate real timing delays.  
- It checks whether the BabySoC design operates correctly when actual cell delays and interconnect delays are considered.  
- This step validates the timing integrity of the SoC under practical conditions.

### 🔍 2. Functional Validation After Synthesis
- Ensures that the design’s logical behavior remains consistent after the synthesis mapping process.  
- Detects possible issues such as glitches, race conditions, or metastability that might not appear during RTL simulation.  
- Verifies that the gate-level representation matches the intended functionality.

### ⚙️ 3. Simulation Environment
- GLS can be performed using simulators like Icarus Verilog or equivalent tools.  
- The resulting waveforms are viewed and analyzed through GTKWave for verification.  
- The process involves compiling the synthesized netlist along with the required models and constraints.

### 🧠 4. Importance for BabySoC
- BabySoC integrates several modules such as the RISC-V core, PLL, DAC, and other subsystems.  
- Gate-Level Simulation confirms that these modules interact correctly and maintain their expected behavior after synthesis.  
- It also validates that the timing constraints and inter-module communication are properly met in the final netlist.

---

## Step-by-Step Plan for Manual Execution
A structured sequence of commands is used to compile, simulate, and observe the results of the synthesized design.  
This involves preparing the gate-level netlist, applying SDF timing information, running the simulation, and finally analyzing the waveform outputs for verification.

---
### **Step 1: Open Synthesis tool & Load the Design Modules**
```bash
yosys
```

Inside the Yosys shell, run:
```yosys
read_verilog /home/vivek/VSDBabySoC/src/module/vsdbabysoc.v
read_verilog -I /home/vivek/VSDBabySoC/src/include/home/vivek/VSDBabySoC/src/module/rvmyth.v
read_verilog -I /home/vivek/VSDBabySoC/src/include /home/vivek/VSDBabySoC/src/module/clk_gate.v

```
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/yosys_read_v.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/read_verilog.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>

---

### **Step 2: Load the Liberty Files for Synthesis**
Inside the same Yosys shell, run:
```yosys
read_liberty -lib /home/vivek/VSDBabySoC/src/lib/avsddac.lib 
read_liberty -lib /home/vivek/VSDBabySoC/src/lib/avsdpll.lib 
read_liberty -lib /home/vivek/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/read_lib.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>

---

### **Step 3: Run Synthesis Targeting `vsdbabysoc`**
```yosys
synth -top vsdbabysoc
```
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/synth.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/clk_gate.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/rvmyth.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/soc.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/hierarchy.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/check_pass.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
---

### **Step 4: Map D Flip-Flops to Standard Cells**
```yosys
dfflibmap -liberty /home/vivek/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

### **Step 5: Perform Optimization and Technology Mapping**
```yosys
opt
abc -liberty /home/vivek/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/abc.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>

---

### **Step 6: Perform Final Clean-Up and Renaming**
```yosys
flatten
setundef -zero
clean -purge
rename -enumerate
```
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/flatten.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
---

### **Step 7: Check Statistics**
```yosys
stat
```
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/stat1.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
<p align="center">
  <img src="https://github.com/vivek-kosigi/RTL2GDS_VSD/blob/main/Week_3/task_1/images/stat2.png" 
       alt="Simulation Block Diagram" width="600"/>
</p>
---

### **Step 8: Write the Synthesized Netlist**
```yosys
write_verilog -noattr /home/vivek/VSDBabySoC/output/post_synth_sim/vsdbabysoc.synth.v
```

---

## POST_SYNTHESIS SIMULATION AND WAVEFORMS
---

### **Step 1: Compile the Netlist woith testbench**
Run the following `iverilog` command to compile:
```bash
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 -o ./output/post_synth_sim/post_synth_sim.out ./src/gls_model/primitives.v ./src/gls_model/sky130_fd_sc_hd.v ./output/post_synth_sim/vsdbabysoc.synth.v ./src/module/avsdpll.v ./src/module/avsddac.v ./src/module/testbench.v
```
---
### **Step 2: Navigate to the Post-Synthesis Simulation Output Directory**
```bash
cd output/post_synth_sim/
```
---
### **Step 3: Run the Simulation**

```bash
./post_synth_sim.out
```
---
### **Step 4: View the Waveforms in GTKWave**

```bash
gtkwave dump.vcd
```
<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%203/Task%201/read_verilog.png" 
       alt="Simulation Block Diagram" width="600"/>

---
