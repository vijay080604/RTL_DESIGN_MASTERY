# Day 1 – RTL Design Flow (Simulation to Synthesis)

## Table of Contents

* Introduction
* Topic 1 – RTL Design
* Topic 2 – Simulation Flow
* Lab Section
* Commands and Execution Flow
* Outputs and Explanation
* Key Takeaways
* Conclusion

---

## Introduction

In this lab, I worked on understanding the complete RTL design flow starting from writing Verilog code to simulation and then converting it into gate-level logic using synthesis.

Instead of only going through theory, I executed each step and observed how the design behaves during simulation and synthesis.

---

## Topic 1 – RTL Design

### What is it

RTL design represents digital circuits using registers and logical operations.

---

### Explanation

RTL describes how data flows and how logic is applied using Verilog. Instead of directly designing gates, we write behavioral code. The synthesis tool later converts this into hardware using standard cell libraries. For example, a conditional statement in Verilog can be mapped into a multiplexer in hardware. Writing correct RTL is important because it directly affects timing, area, and correctness of the final design.

---

### Importance

* base of digital design
* affects synthesis output
* ensures correct functionality

---

### Usage in Lab

Used RTL to design a 2:1 multiplexer.

---

## Topic 2 – Simulation Flow

### What is it

Simulation is used to verify the design before synthesis.

---

### Explanation

Simulation checks whether the design behaves correctly for different inputs. I used Icarus Verilog to compile and run the design along with a testbench. A VCD file is generated which stores signal transitions. This file is opened in GTKWave to observe waveforms. This step helps in debugging and confirming logic correctness before moving to synthesis.

---

### Importance

* verifies logic
* helps debugging
* prevents errors

---

### Usage in Lab

Simulated mux design and verified waveform.

---

## Lab Section

### Objective of the Code

To design and verify a 2:1 multiplexer and observe its behavior.

---

### Design Code

```verilog
module good_mux (input i0 , input i1 , input sel , output reg y);

always @ (*)
begin
    if(sel)
        y <= i1;
    else
        y <= i0;
end

endmodule
```

---

### Testbench Code

```verilog
`timescale 1ns / 1ps

module tb_good_mux;

reg i0, i1, sel;
wire y;

good_mux uut (.i0(i0), .i1(i1), .sel(sel), .y(y));

initial begin
    $dumpfile("tb_good_mux.vcd");
    $dumpvars(0, tb_good_mux);

    i0 = 0; i1 = 0; sel = 0;
    #300 $finish;
end

always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;

endmodule
```

---

## Commands and Execution Flow

### Step 1 – Compile

```bash
iverilog good_mux.v tb_good_mux.v
```

Explanation:
Compiles design and testbench, checks syntax, generates executable.

---

### Step 2 – Run Simulation

```bash
./a.out
```

Explanation:
Executes simulation and generates VCD waveform file.

---

### Step 3 – View Waveform

```bash
gtkwave tb_good_mux.vcd
```

Explanation:
Opens waveform viewer to analyze signals.

---

### Complete Flow

```bash
iverilog good_mux.v tb_good_mux.v
./a.out
gtkwave tb_good_mux.vcd
```

---

## Outputs and Explanation

### RTL Design

![MUX Design](./screenshots/01_mux_design.png)

Explanation:
Shows Verilog implementation of mux.

---

### Testbench

![Testbench](./screenshots/02_testbench.png)

Explanation:
Generates different input combinations.

---

### Waveform Output

![Waveform](./screenshots/03_waveform.png)

Explanation:

* sel = 0 → output follows i0
* sel = 1 → output follows i1

---

## Synthesis Outputs

### Synthesis Command Flow

```bash
yosys
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v
synth -top good_mux
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

---

### Synthesis

![Yosys Synthesis](./screenshots/06_yosys_synthesis.png)

Explanation:
RTL converted into gate-level logic.

---

### Netlist

![Netlist](./screenshots/07_netlist.png)

Explanation:
Shows logic connections.

---

### Netlist View

![Netlist View](./screenshots/08_netlist_view.png)

Explanation:
Graphical representation of mux.

---

## Key Takeaways

* understood RTL design
* learned simulation flow
* understood commands clearly
* observed synthesis
* verified output

---

## Conclusion

This lab helped me understand how RTL design is simulated and converted into hardware using synthesis tools.
