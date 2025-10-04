## Contents
- [Task1-Fundamentals of SoC Design](#Task1-Fundamentals-of-SoC-Design)
- [Task2-VSDBabySoC Project Pre_synth_sim](#Task2-VSDBabySoC-Project-Pre_synth_sim)

# Task1-Fundamentals of SoC Design

## Objective

Develop a solid understanding of System-on-Chip (SoC) fundamentals and apply them by modelling BabySoC which is a compact, documented open-source SoC—using simulation tools. This project explores integration of CPU, clock generation, and digital-to-analog conversion, targeting digital-analog interfacing and experimentation.

---

## What is a System-on-Chip (SoC)?

A System-on-Chip, or SoC, is essentially a complete computer system miniaturized onto a single chip. SoCs combine the CPU, memory, input/output interfaces, graphics processors, and other specialized circuits, all in one compact design. This architecture is widely used in smartphones, tablets, wearables, and many embedded devices because it saves space, reduces power consumption, and increases performance.

### Key Components

- **CPU (Central Processing Unit)**: The main brain, handling instructions, data processing, and decision-making.
- **Memory (RAM/ROM)**: Includes RAM for active processes and flash or ROM for permanent data storage.
- **Input/Output Ports**: Interface for connecting peripherals (cameras, USB, audio, etc.).
- **GPU (Graphics Processing Unit)**: Generates display visuals and handles animations.
- **DSP (Digital Signal Processor)**: Processes audio/video signals for high-performance media tasks.
- **Power Management**: Regulates energy usage and extends battery life.
- **Connectivity & Security**: Integrates modules for Wi-Fi, Bluetooth, and data protection.

### Benefits of SoCs

- **Compact Design**: Enables powerful features in small devices.
- **Energy Efficiency**: Reduces power consumption - a must for wearables and handhelds.
- **Performance**: Short internal connections boost speeds.
- **Reliability & Cost**: Fewer discrete components lower complexity and risk.

---

### Types of SoCs

- **Microcontroller-based**: Designed for simple control tasks in everyday devices.
- **Microprocessor-based**: Power demanding, multitasking gadgets like smartphones and tablets.
- **Application-Specific**: Customized for dedicated high-performance tasks (e.g., AI or graphics cards).

---

## SoC Design Flow Overview

Designing an SoC is a detailed, step-by-step process to bring together many components onto one chip. Here is an approachable look at the typical design flow:
It is generally divided into Front-End (hardware design) and Software Development tracks, which are closely integrated to ensure that the final product works as a single, unified system.

<p align="center">
  <img src="https://github.com/lagudushruthi/Risc-V-RTL2GDS/blob/main/Week2/SoC design flow.png" 
       alt="SoC design flow" width="600"/>
</p>


## 1. SoC Specification

The journey begins by clearly defining objectives, performance requirements, interfaces, and constraints for the proposed SoC. The specification sets the foundation for all subsequent design choices.


## 2. Architecture Design (Hardware/Software Partitioning)

At this point, decisions are made about which system elements will be implemented in hardware and which in software. This architectural blueprint ensures maximum performance and cost-efficiency.


## Front-End Design (Hardware)

### High Level Modeling

Engineers create abstract representations capturing essential data paths, control signals, and interactions among modules. This model visualizes capabilities and pinpoints possible bottlenecks.

### RTL Design

The SoC logic is described at the Register Transfer Level (RTL) using hardware description languages like Verilog. RTL design defines precise timing and data handling at each clock cycle.

### Functional Simulation and Verification

Simulation tools validate that the RTL logic matches the intended specification. Bugs and logic errors are fixed here before moving forward.

### RTL Synthesis and DFT

RTL code is synthesized into a detailed gate-level netlist. Design for Testability (DFT) features are added to facilitate easier testing after fabrication.

### Gate Level Netlist

The netlist serves as the physical circuit blueprint for further implementation.


## Physical Design (Back-End)

### Place and Route

Physical placement of gates and wires is optimized to fit design and manufacturing constraints.

### Timing Verification and Signoff

Analyzes timing to ensure the chip will operate at the desired clock speed and meets all critical path requirements.

### Physical Verification

The layout is checked for manufacturability, compliance with design rules, and absence of unintended shorts or opens.

### Design GDSII

Finalized layout is exported as a GDSII file for chip fabrication.

### Manufacturing

The design is sent to a semiconductor foundry for fabrication.

### Post-Silicon Validation and Integration

Early silicon is tested under real-world conditions, and hardware-software integration is validated. Any hardware or software issues are addressed before scaling.

### Mass Production

Fully validated chips enter volume manufacturing.

## Software Development Flow

- **Software Development:** Code like drivers, operating system support, and applications are created in parallel with hardware progress.
- **Software Testing and Refinement:** Continual testing and refinement ensure compatibility with evolving hardware.
- **Final Code:** Mature software is integrated after thorough verification.

## HW/SW Co-Simulation and Integration

Hardware and software are co-simulated throughout the process to ensure system-level consistency, catch interface mismatches early, and enable smooth integration at the end.


## Summary

By systematically iterating through specification, design, verification, and integration—using both hardware and software co-simulation—the SoC design flow greatly increases the chances of building a robust, high-performing, and manufacturable chip.


---

## VSDBabySoC Architecture

VSDBabySoC is a compact, open-source SoC built around the RISC-V RVMYTH processor core. It is designed to demonstrate digital-analog co-design and seamless integration of modern SoC features.

<p align="center">
  <img src="https://github.com/lagudushruthi/Risc-V-RTL2GDS/blob/main/Week2/VSDBabySoC.png" 
       alt="VSDBabySoC" width="600"/>
</p>

### Key Modules

- **RVMYTH (RISC-V CPU)**
  - The heart of BabySoC, RVMYTH implements a reduced instruction set computing (RISC) architecture.
  - Core features include a load-store approach to memory access and a minimal instruction set.
  - RVMYTH continuously cycles the r17 register to hold output data, providing values for peripheral modules.
    
- **Phase-Locked Loop (PLL)**
  - Provides precision clock signals synchronized to a reference input.
  - Minimizes jitter and timing errors, ensuring all system blocks operate harmoniously.
  - Essential for high-speed and reliable communication between chip modules.

- **10-bit Digital-to-Analog Converter (DAC)**
  - Converts digital output from the CPU into 1024 distinct analog voltage levels.
  - Facilitates direct interfacing with analog hardware, such as speakers or displays.
  - Demonstrates real multimedia output from a digital core.

### System Operation

- At startup, the PLL generates and distributes a stable clock.
- The RVMYTH CPU runs instructions, updating `r17` with digital output data.
- The DAC reads these values and produces a continuous analog waveform, which is saved as a file (OUT) or delivered to external analog devices.

---

# Task2-VSDBabySoC Project Pre_synth_sim

## Objective

To understand SoC fundamentals and demonstrate BabySoC functional modelling using Icarus Verilog and GTKWave simulation tools.

---
## Project Structure
- `src/include/` - Contains header files (`*.vh`) with necessary macros or parameter definitions.
- `src/module/` - Contains Verilog files for each module in the SoC design.
- `output/` - Directory where compiled outputs and simulation files will be generated.

## Set-up Guide
Clone or set up the directory structure as follows:
```txt
VSDBabySoC/
├── src/
│   ├── include/
│   │   ├── sandpiper.vh
│   │   └── other header files...
│   ├── module/
│   │   ├── vsdbabysoc.v      # Top-level module integrating all components
│   │   ├── rvmyth.v          # RISC-V core module
│   │   ├── avsdpll.v         # PLL module
│   │   ├── avsddac.v         # DAC module
│   │   └── testbench.v       # Testbench for simulation
└── output/
        ├── pre_synth_sim
        └── pre_synth_sim.out
```
---
## Workflow

1. **Cloning the Project**
    ```
    git clone https://github.com/manili/VSDBabySoC.git
    ```

2. **Convert rvmyth.tvl to rvmyth.v using sandpiper-saas**

   ```
   sudo apt install python3-venv python3-pip
   python3 -m venv sp_env
   source sp_env/bin/activate
   pip install pyyaml click sandpiper-saas
   ```
   ***Convert from .tlv to .v***
   ````
   sandpiper-saas -i ./src/module/*.tlv -o rvmyth.v --bestsv --noline -p verilog --outdir ./src/module/

2. **Compilation and Simulation**
    ```
    mkdir -p output/pre_synth_sim
    
    iverilog -o output/pre_synth_sim/pre_synth_sim.out \
      -DPRE_SYNTH_SIM \
      -I src/include -I src/module/ \
      src/module/testbench.v
    cd output/pre_synth_sim
    
    ./pre_synth_sim.out
    ```

3. **Launching GTKWave**

    ```
    gtkwave pre_synth_sim.vcd
    ```

---

## Simulation Waveform

<p align="center">
  <img src="https://github.com/lagudushruthi/Risc-V-RTL2GDS/blob/main/Week2/pre_synth_sim.png" 
       alt="pre_synth_sim" width="600"/>
</p>



---

## Simulation Analysis

> *Outputs are initialized during reset (`reset=1`). Once reset is de-asserted, signals show active values, confirming correct design initialization.*

---

### Clock Behavior


---

### Data Flow Between Modules

> *Transition of `TO_DAC[9:0]` from the core to the DAC followed by change in `OUT`, shows proper data movement.*
---

## Summary

All steps from testbench compilation to functional waveform analysis are verified, showing correct BabySoC SoC reset, clocking, module interfacing, and register operations.





