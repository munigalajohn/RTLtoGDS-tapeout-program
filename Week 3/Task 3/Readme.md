
## Installation of OpenSTA

### **Clone the Repository**

```bash
git clone https://github.com/parallaxsw/OpenSTA.git
cd OpenSTA
```

### **Build the Docker Image**

```bash
docker build --file Dockerfile.ubuntu22.04 --tag opensta .
```

### **Run the OpenSTA Container**

```bash
docker run -i -v $HOME:/data opensta
```
% symbol indicates the opensta shell.

---

# **VSDBabySoC — Basic Timing Analysis with OpenSTA**

**Steps for Static Timing Analysis (STA)** of the **VSDBabySoC** design using **OpenSTA**

---

## **1. Prepare Required Files**

```bash
# Create a directory to store Liberty timing libraries
/Desktop/VSDBabySoC/OpenSTA$ mkdir -p examples/timing_libs/
/Desktop/VSDBabySoC/OpenSTA/examples$ ls timing_libs/
avsddac.lib  avsdpll.lib  sky130_fd_sc_hd__tt_025C_1v80.lib

# Create a directory to store synthesized netlist and constraint files
/Desktop/VSDBabySoC/OpenSTA$ mkdir -p examples/BabySoC
/Desktop/VSDBabySoC/OpenSTA/examples$ ls BabySoC/
gcd_sky130hd.sdc  vsdbabysoc_synthesis.sdc  vsdbabysoc.synth.v
```


**File Overview**

*  **Standard Cell Library:** `sky130_fd_sc_hd__tt_025C_1v80.lib`
*  **IP-Specific Libraries:** `avsdpll.lib`, `avsddac.lib`
*  **Synthesized Netlist:** `vsdbabysoc.synth.v`
*  **Timing Constraints:** `vsdbabysoc_synthesis.sdc`

---


## **2. Run Min/Max Delay Timing Checks**

Below is the TCL script to perform **complete min/max timing analysis** on the SoC:

vsdbabysoc_min_max_delays.tcl

```shell
read_liberty -min /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib
read_liberty -max /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib

read_liberty -min /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/avsdpll.lib
read_liberty -max /data/Desktopvsd/VLSI/VSDBabySoC/OpenSTA/examples/timing_libs/avsdpll.lib

read_liberty -min /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/avsddac.lib
read_liberty -max /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/avsddac.lib

read_verilog /Desktop/VSDBabySoC/OpenSTA/examples/BabySOC/vsdbabysoc.synth.v
link_design vsdbabysoc

read_sdc /Desktop/VSDBabySoC/OpenSTA/examples/BabySOC/vsdbabysoc_synthesis.sdc

report_checks
```


| 🔧 **Command**  | 📝 **Purpose**      | 💡 **Description**                                                                    |
| --------------- | ------------------- | ------------------------------------------------------------------------------------- |
| `read_liberty`  | Load .lib files     | Loads **standard cell and IP libraries** for both min (hold) and max (setup) analysis |
| `read_verilog`  | Read netlist        | Loads the **synthesized gate-level design**                                           |
| `link_design`   | Link top module     | Sets `vsdbabysoc` as the **top-level module**                                         |
| `read_sdc`      | Read constraints    | Loads **clock, delay, and exception constraints**                                     |
| `report_checks` | Run timing analysis | Generates **setup and hold timing reports**                                           |

---

### **Execute Inside Docker**

```bash
docker run -it -v $HOME:/data opensta /data/Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/vsdbabysoc_min_max_delays.tcl
```

---

## ⚠️ **3. Common Errors & Fix**

If you encounter this error:

```shell
Warning: /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib line 23, default_fanout_load is 0.0.
Warning: /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/sky130_fd_sc_hd__tt_025C_1v80.lib line 1, library already exists.
Error: /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/avsdpll.lib line 54, syntax error
```

**Cause:**
Liberty format doesn’t allow `//` for comments — it must use `/* ... */`.

❌ Incorrect:

```shell
//pin (GND#2) {
//  direction : input;
//  max_transition : 2.5;
//  capacitance : 0.001;
//}
```

✅ Correct:

```shell
/*
pin (GND#2) {
  direction : input;
  max_transition : 2.5;
  capacitance : 0.001;
}
*/
```

Fixing this syntax will resolve the parsing error and allow OpenSTA to continue analysis smoothly.

<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%203/Task%203/report_checks.png" 
       alt="SoC design flow" width="600"/>
</p>

 - Positive slack of 0.79 means timing requirements are met (setup check is successful).


## **4. VSDBabySoC PVT Corner Analysis (Post-Synthesis Timing)**

STA is performed across **Process–Voltage–Temperature (PVT)** corners to ensure robustness under all conditions.

### **Setup-Critical (Max Path) Corners**

* `ss_LowTemp_LowVolt`
* `ss_HighTemp_LowVolt`
  Represent **slowest** operating conditions.

### **Hold-Critical (Min Path) Corners**

* `ff_LowTemp_HighVolt`
* `ff_HighTemp_HighVolt`
  Represent **fastest** operating conditions.

## **5. Automated STA Across All PVT Corners**

sta_across_pvt.tcl

```shell
 # List of standard cell liberty files
set list_of_lib_files(1) "sky130_fd_sc_hd__tt_025C_1v80.lib"
set list_of_lib_files(2) "sky130_fd_sc_hd__ff_100C_1v65.lib"
set list_of_lib_files(3) "sky130_fd_sc_hd__ff_100C_1v95.lib"
set list_of_lib_files(4) "sky130_fd_sc_hd__ff_n40C_1v56.lib"
set list_of_lib_files(5) "sky130_fd_sc_hd__ff_n40C_1v65.lib"
set list_of_lib_files(6) "sky130_fd_sc_hd__ff_n40C_1v76.lib"
set list_of_lib_files(7) "sky130_fd_sc_hd__ss_100C_1v40.lib"
set list_of_lib_files(8) "sky130_fd_sc_hd__ss_100C_1v60.lib"
set list_of_lib_files(9) "sky130_fd_sc_hd__ss_n40C_1v28.lib"
set list_of_lib_files(10) "sky130_fd_sc_hd__ss_n40C_1v35.lib"
set list_of_lib_files(11) "sky130_fd_sc_hd__ss_n40C_1v40.lib"
set list_of_lib_files(12) "sky130_fd_sc_hd__ss_n40C_1v44.lib"
set list_of_lib_files(13) "sky130_fd_sc_hd__ss_n40C_1v76.lib"

# Read custom libs first
read_liberty /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/avsdpll.lib
read_liberty /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/avsddac.lib

for {set i 1} {$i <= [array size list_of_lib_files]} {incr i} {

    read_liberty /data/Desktop/VSDBabySoC/OpenSTA/examples/timing_libs/$list_of_lib_files($i)
    read_verilog /data/Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/vsdbabysoc.synth.v
    link_design vsdbabysoc
    current_design
    read_sdc /data/Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/vsdbabysoc_synthesis.sdc

    check_setup -verbose

    report_checks -path_delay min_max -fields {nets cap slew input_pins fanout} -digits {4} \
        > /data/Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/min_max_$list_of_lib_files($i).txt

    exec echo "$list_of_lib_files($i)" >> /data/vsd/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_max_slack.txt
    report_worst_slack -max -digits {4} >> /data/Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_max_slack.txt

    exec echo "$list_of_lib_files($i)" >> /data/vsd/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_min_slack.txt
    report_worst_slack -min -digits {4} >> /data/Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_worst_min_slack.txt

    exec echo "$list_of_lib_files($i)" >> /data/vsd/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_tns.txt
    report_tns -digits {4} >> /data/Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_tns.txt

    exec echo "$list_of_lib_files($i)" >> /data/vsd/VLSI/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_wns.txt
    report_wns -digits {4} >> /data/Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT/sta_wns.txt
}

```


|  **Command**             |  **Purpose**          | **Explanation**                                |
| ------------------------- | -------------------- | ------------------------------------------------- |
| `report_worst_slack -max` | Worst Setup Slack    | Finds the **most negative setup slack (WNS)**     |
| `report_worst_slack -min` | Worst Hold Slack     | Finds the **most negative hold slack**            |
| `report_tns`              | Total Negative Slack | Sums all violations (indicates design robustness) |
| `report_wns`              | Worst Negative Slack | Shows single **worst-case violation**             |


### **Run the Automated Script**

```shell
docker run -it -v $HOME:/data opensta /data/Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/sta_across_pvt.tcl
```


**Generated Output Directory:**

```shell
Desktop/VSDBabySoC/OpenSTA/examples/BabySoC/STA_OUTPUT$ ls
min_max_sky130_fd_sc_hd__ss_100C_1v80.lib.txt
sta_worst_max_slack.txt
sta_worst_min_slack.txt
sta_tns.txt
sta_wns.txt
```

| **File Name**             | **Description**                         |
| ------------------------- | ------------------------------------------ |
| `min_max_<lib>.txt`       | Detailed timing report for each PVT corner |
| `sta_worst_max_slack.txt` | Worst setup slack across corners           |
| `sta_worst_min_slack.txt` | Worst hold slack across corners            |
| `sta_tns.txt`             | Total Negative Slack summary               |
| `sta_wns.txt`             | Worst Negative Slack summary               |

---


<p align="center">
  <img src="https://github.com/munigalajohn/RTLtoGDS-tapeout-program/blob/main/Week%203/Task%203/sta_output.png" 
       alt="SoC design flow" width="600"/>
</p>

