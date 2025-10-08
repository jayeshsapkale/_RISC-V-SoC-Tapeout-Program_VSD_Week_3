# Week 3 Part:1 Post-Synthesis GLS & STA Fundamentals

## Objective: To understand and perform Gate-Level Simulation (GLS) after synthesis, validatefunctionality, and get introduced to Static Timing Analysis (STA) concepts with practicalexperiments using OpenSTA.

# Step 1: Yosys Synthesis
Navigate to project folder:
```
cd ~/VLSI/VSDBabySoC
```
Launch Yosys:
```
yosys
```
Inside Yosys, run:

# Read Verilog modules
```
read_verilog /home/vboxuser/VLSI/VSDBabySoC/src/module/vsdbabysoc.v
read_verilog -I /home/vboxuser/VLSI/VSDBabySoC/src/include /home/vboxuser/VLSI/
VSDBabySoC/src/module/rvmyth.v
read_verilog -I /home/vboxuser/VLSI/VSDBabySoC/src/include /home/vboxuser/VLSI/
VSDBabySoC/src/module/clk_gate.v
```

<img width="940" height="589" alt="image" src="https://github.com/user-attachments/assets/ba2bf292-c326-4367-86ab-f70d38a52999" />

# Read Liberty standard cell libraries
```
read_liberty -lib /home/vboxuser/VLSI/VSDBabySoC/src/lib/avsdpll.lib
read_liberty -lib /home/vboxuser/VLSI/VSDBabySoC/src/lib/avsddac.lib
read_liberty -lib /home/vboxuser/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

<img width="940" height="591" alt="image" src="https://github.com/user-attachments/assets/a13dd9c5-6365-42ff-9fad-933fd273ff16" />

# Run synthesis tragetting vsdbabysoc:
```
synth -top vsdbabysoc
```

<img width="940" height="588" alt="image" src="https://github.com/user-attachments/assets/f753b388-68d4-47c0-b87a-6162d09787b8" />

# Map D Flip-Flops to Standard Cells
```
dfflibmap -liberty /home/vboxuser/VLSI/VSDBabySoC/src/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```
<img width="940" height="585" alt="image" src="https://github.com/user-attachments/assets/a4ba28fe-e739-4d74-aedd-e6fe1a9e6293" />


# Optimize and map design
```
opt
abc -liberty /home/vboxuser/VLSI/VSDBabySoC/src/lib/
sky130_fd_sc_hd__tt_025C_1v80.lib -script +strash;scorr;ifraig;retime;
{D};strash;dch,-f;map,-M,1,{D}
```

<img width="940" height="546" alt="image" src="https://github.com/user-attachments/assets/8f11cf6e-f7bc-45f6-9833-aebe59567514" />
<img width="940" height="587" alt="image" src="https://github.com/user-attachments/assets/18619194-a3e0-45d1-9a8c-1d614e13f879" />

# Perform Final Clean-Up and Renaming
```
flatten
setundef -zero
clean -purge
rename -enumerate
```
<img width="940" height="585" alt="image" src="https://github.com/user-attachments/assets/7e9050d4-90b8-4a9c-9a56-658e17096a9b" />

# Write synthesized netlist
```
write_verilog -noattr /home/vboxuser/VLSI/VSDBabySoC/src/gls_model/
vsdbabysoc.synth.v
```
<img width="940" height="589" alt="image" src="https://github.com/user-attachments/assets/30c2e75d-194d-4e15-aba7-9d0ef6f22033" />

# Exit Yosys
```
exit
```
# Step 2: Create GLS output folder
```
mkdir -p /home/vboxuser/VLSI/VSDBabySoC/output/post_synth_sim
```
# Step 3: Compile Gate-Level Simulation (GLS)
```
iverilog -o /home/vboxuser/VLSI/VSDBabySoC/output/post_synth_sim/
post_synth_sim.out
-DPOST_SYNTH_SIM -DFUNCTIONAL -DUNIT_DELAY=#1
-I /home/vboxuser/VLSI/VSDBabySoC/src/include
-I /home/vboxuser/VLSI/VSDBabySoC/src/module
-I /home/vboxuser/VLSI/VSDBabySoC/src/gls_model
/home/vboxuser/VLSI/VSDBabySoC/src/module/testbench.v
```


# Step 4: Run GLS Simulation
```
vvp /home/vboxuser/VLSI/VSDBabySoC/output/post_synth_sim/post_synth_sim.out
```

This executes the gate-level simulation and generates the VCD waveform file (if $dumpfile is used
in the testbench).

# Step 5: View Waveforms in GTKWave
```
gtkwave /home/vboxuser/VLSI/VSDBabySoC/src/module/pre_synth_sim.vcd
```
<img width="940" height="584" alt="image" src="https://github.com/user-attachments/assets/05cffa02-cc2f-4ed5-b593-54e0fe4acffc" />

# POST_SYNTHESIS SIM WAVEFORMS

<img width="940" height="584" alt="image" src="https://github.com/user-attachments/assets/1886eed8-6aec-4cd2-b38d-21e961f9615d" />

# PRE_SYNTHESIS SIM WAVEFORMS
<img width="940" height="595" alt="image" src="https://github.com/user-attachments/assets/4d487e4c-080b-4952-b8eb-6b2621abc0a5" />


## Compare GLS waveforms with functional simulation waveforms to verify correctness.
## Gate-Level Simulation (GLS) vs Functional Simulation Comparison

### 🔍 Summary
I inspected the two waveform screenshots — **pre-synthesis (functional simulation)** and **post-synthesis (GLS)** — for the **BabySoC** design.

- The key signals **`OUT`** and **`D[9:0]`** show **identical transitions and timing alignment** in both captures.  
- The analog real traces (**`VREFH`**, **`VREFL`**, and **`OUT`**) also exhibit the **same shape and timing window** in both waveforms.  
- No mismatches or unexpected delays were observed between the functional and GLS results.

### ✅ Conclusion
For the observed signals and time window:
> **GLS output matches the Functional simulation output.**
