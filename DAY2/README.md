# 🚀 Day 2 – Timing Libraries, Hierarchical vs Flat Synthesis & Flop Coding Styles

---

# 📌 Introduction

In Day 2, I moved beyond basic RTL coding and started understanding how digital designs are actually interpreted and implemented by synthesis tools. The focus was not only on writing Verilog code, but also on understanding how that code is translated into real hardware using timing libraries and optimization techniques.

This session provided clarity on how tools like Yosys convert abstract RTL descriptions into gate-level implementations, and how design decisions such as hierarchy and coding styles directly affect the final hardware in terms of performance, area, and power.

---

# 🔹 1. Timing Libraries (.lib)

---

## 📖 What is a Timing Library?

A timing library file (commonly referred to as a `.lib` file) is a standardized format used in digital design that contains detailed information about all the standard cells available for synthesis. These standard cells include basic logic gates such as AND, OR, NAND, NOR, as well as more complex elements like flip-flops and buffers.

The `.lib` file does not contain the actual circuit design; instead, it contains characterization data about each cell, including its delay, power consumption, and physical area.

---

## 🧠 SKY130 PDK Overview

The SKY130 PDK (Process Design Kit) is an open-source technology provided by SkyWater based on 130nm CMOS technology.

It provides all the necessary data required to design integrated circuits, including:

* Standard cell libraries
* Timing information
* Power characteristics
* Process variation data

---

## 🔍 Understanding Library Naming

Example:

```text id="libnaming"
sky130_fd_sc_hd__tt_025C_1v80.lib
```

### 🧠 Breakdown

* sky130 → Technology node (130nm)
* fd_sc_hd → Standard cell library (high density)
* tt → Typical process corner
* 025C → Temperature = 25°C
* 1v80 → Voltage = 1.8V

---

## 🧠 What is PVT (Process, Voltage, Temperature)?

### 🔸 Process

Manufacturing variations (FF, SS, TT)

### 🔸 Voltage

Supply variation affects speed

### 🔸 Temperature

Higher temperature slows circuits

---

## 🔥 Key Insight

The same RTL design behaves differently under different PVT conditions. Hence, multiple libraries exist for different scenarios.

---

## 🔧 Command Used

```bash id="cmd_lib_full"
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
```

---

## 🧠 What This Command Does

* Loads all standard cells into Yosys
* Provides delay, power, and area data
* Enables mapping from RTL to real hardware

---

## 📸 From My Execution

![Yosys Read](screenshots/yosys_read.png)

---

## 🧠 What I Observed Inside .lib

* `cell()` → defines gates
* `pin()` → defines I/O
* `area` → size
* `leakage_power` → power
* `timing()` → delay

---

## 🚀 Final Understanding

👉 `.lib` is the bridge between **RTL and silicon**

---

# 🔹 2. Hierarchical vs Flat Synthesis

---

## 📖 Hierarchical Design

Design is divided into modules for better readability and reuse.

---

## 📖 Flat Design

All modules merged into a single level.

---

## 🔧 Commands Used

```bash id="cmd_flat"
flatten
write_verilog multiple_module_flat.v
```

---

## 🧠 Explanation

* flatten → removes hierarchy
* write_verilog → saves synthesized design

---

## 🔥 Insight

Hierarchy is useful for humans, while flattening helps tools optimize better.

---

# 🔹 3. RTL Design Using Multiple Modules

---

## 💻 Code

```verilog id="cmd_rtl"
wire net1;

sub_module1 u1 (.a(a), .b(b), .y(net1));
sub_module  u2 (.a(net1), .b(c), .y(y));
```

---

## 🧠 Understanding

Signals flow through wires connecting modules, forming real hardware structure.

---

## 📸 RTL View

![RTL View](screenshots/rtl_view.png)

---

# 🔹 4. RTL vs Netlist

---

## 📖 RTL

High-level behavioral description.

---

## 📖 Netlist

Gate-level implementation.

---

## 📸 Comparison

![RTL vs Netlist](screenshots/rtl_vs_netlist.png)

---

## 🧠 Insight

RTL defines functionality, netlist defines implementation.

---

# 🔹 5. Synthesis Flow (Detailed)

---

## 🔧 Commands

```bash id="cmd_flow"
yosys

read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_module.v

synth -top multiple_module

abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib

write_verilog multiple_module_netlist.v
```

---

## 🧠 Step-by-Step

* yosys → start tool
* read_liberty → load library
* read_verilog → load RTL
* synth → convert to logic
* abc → map to gates
* dfflibmap → map flip-flops
* write_verilog → generate netlist

---

## 📸 Output

![Yosys Output](screenshots/yosys_read.png)

---

# 🔹 6. Sequential Logic – Flip-Flops

---

## 📖 Why Needed

Combinational logic cannot store data and may produce unstable outputs.

---

## 💻 Code

```verilog id="cmd_dff"
always @(posedge clk or posedge reset)
begin
    if (reset)
        q <= 1'b0;
    else
        q <= 1'b1;
end
```

---

## 📸 Mapping

![DFF Mapping](screenshots/dff_mapping.png)

---

## 🧠 Insight

Flip-flops introduce memory and synchronization into digital systems.

---

# 🔹 7. Simulation Flow

---

## 🔧 Commands

```bash id="cmd_sim"
iverilog dff_const.v tb_dff_const.v
./a.out
gtkwave tb_dff_const.vcd
```

---

## 📸 Waveform

![Waveform](screenshots/waveform.png)

---

## 🧠 Understanding

Simulation verifies correctness before hardware implementation.

---

# 🔹 8. Key Insights

---

* RTL is abstract
* `.lib` defines real hardware
* Synthesis converts logic to gates
* Coding style affects optimization
* PVT affects circuit behavior

---

# 📊 Summary

| Topic             | Status |
| ----------------- | ------ |
| Timing Libraries  | ✅      |
| PVT Concepts      | ✅      |
| Hierarchy vs Flat | ✅      |
| RTL Design        | ✅      |
| Synthesis Flow    | ✅      |
| Flip-Flops        | ✅      |
| Simulation        | ✅      |

---

# 🚀 Final Conclusion

Day 2 provided a deep understanding of how digital designs are transformed from RTL descriptions into real hardware using synthesis tools. It emphasized the role of timing libraries, PVT conditions, and design structure in achieving efficient and optimized circuits.

---

🔥 This forms the core foundation for real-world VLSI design and synthesis
