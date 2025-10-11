# Week 3 Task 3 – Post-Synthesis GLS & STA Fundamentals

## Objective
To understand and perform Gate-Level Simulation (GLS) after synthesis, validate functionality, and get introduced to Static Timing Analysis (STA) concepts using OpenSTA.

---

## 1. OpenSTA Setup

**Commands to run OpenSTA:**

```bash
cd ~/VLSI/VSDBabySoC/OpenSTA/build
./sta
```
## 2. Load Libraries and Netlist
# Read Liberty Timing Library
read_liberty ../../src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib  

# Read Synthesized Verilog Netlist
read_verilog ../../OpenSTA/examples/BabySoC/vsdbabysoc.synth.v  

# Link Design
link_design vsdbabysoc

# Read SDC Constraints
read_sdc ../../OpenSTA/examples/BabySoC/vsdbabysoc_synthesis.sdc]

## 3. Run STA Checks

# Setup Timing Check
```
check_setup -verbose
```
<img width="940" height="582" alt="image" src="https://github.com/user-attachments/assets/093ad8c2-8bb1-493d-bdd0-2ee20403ff38" />

## 4. Generate Timing Reports
```
# Worst Slack
report_worst_slack -max -digits 4 > STA_OUTPUT/sta_worst_max_slack.txt
report_worst_slack -min -digits 4 > STA_OUTPUT/sta_worst_min_slack.txt

# Total Negative Slack (TNS) & Worst Negative Slack (WNS)
report_tns -digits 4 > STA_OUTPUT/sta_tns.txt
report_wns -digits 4 > STA_OUTPUT/sta_wns.txt
```
<img width="1441" height="901" alt="image" src="https://github.com/user-attachments/assets/2c401321-376a-425d-9d20-ad4df0caacc4" />
## 5. Observations

