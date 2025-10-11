
# Week 3 Task 2 – STA Fundamentals Summary

## Static Timing Analysis (STA) Overview
- STA is used to verify the timing of a digital circuit without requiring simulation vectors.
- Ensures that signals propagate correctly through combinational and sequential paths.
- Helps identify setup/hold violations, slack, and critical paths.

---

## Key Concepts

### 1. Timing Checks
- **Setup Check:** Ensures data arrives at a register **before** the clock edge.
- **Hold Check:** Ensures data stays stable at a register **after** the clock edge.
- **Slack:** Difference between required arrival time and actual arrival time.  
  - Positive slack: Timing is met  
  - Negative slack: Timing violation

### 2. Path Types
- **reg2reg:** Register to register paths
- **in2reg:** Input to register paths
- **reg2out:** Register to output paths
- **in2out:** Input to output paths
- **Clock gating checks**
- **Recovery/Removal checks**
- **Data-to-data paths**
- **Latch timing:** Time borrowing / Time given

### 3. Slew/Transition Analysis
- **Data:** Maximum and minimum transition times
- **Clock:** Maximum and minimum transition times

### 4. Load Analysis
- **Fanout:** Maximum and minimum fanout
- **Capacitance:** Maximum and minimum load capacitance

### 5. Clock Analysis
- **Skew:** Difference in arrival time between clock paths
- **Pulse Width:** Duration of the clock signal at the register

---

## Importance
- Identifies critical paths and bottlenecks in timing.
- Ensures reliable operation at target clock frequency.
- Guides design optimization for setup/hold margins and power-performance trade-offs.

