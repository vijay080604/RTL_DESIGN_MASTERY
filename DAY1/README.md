# Day 1 – RTL Design Flow (Hands-on Work)

## Objective

The main aim of this lab was to understand how a digital design moves from writing Verilog code to getting a synthesized gate-level representation.

Instead of just learning theory, I focused on running each step and observing what is actually happening in the tools.

---

## What is RTL (Register Transfer Level)

RTL is a way of describing digital circuits in terms of data flow between registers and the operations performed on that data.

In simple words, instead of thinking about gates directly, we describe:
- What data is coming in
- How it is processed
- What output we should get

This is usually done using Verilog.

One important thing I noticed is that RTL is more about behavior, not the actual hardware implementation.

---

## RTL Design Flow (What I followed)

From what I understood during this lab, the design flow is not just one step, it is a sequence:

1. Write the design in Verilog  
2. Create a testbench to verify it  
3. Run simulation to check correctness  
4. View waveform to understand signal changes  
5. Run synthesis to convert into gates  

Each step depends on the previous one. If simulation is wrong, synthesis is also meaningless.

---

## Tools Used

- Icarus Verilog – for compiling and running simulation  
- GTKWave – for viewing waveform  
- Yosys – for synthesis  
- GitHub Codespaces – where I executed everything  

---

## Step 1 – Writing RTL Code (2:1 MUX)

I implemented a simple 2:1 multiplexer.

A multiplexer selects one input out of multiple inputs based on a select signal.

In this case:
- Inputs: i0, i1  
- Select: sel  
- Output: y  

Working condition:
- sel = 0 → output should follow i0  
- sel = 1 → output should follow i1  

While writing this, I understood that even a simple design needs to be clearly defined, otherwise verification becomes confusing.

---

## Step 2 – Testbench

After writing the design, I created a testbench to verify it.

The purpose of testbench is not to design logic, but to apply inputs and observe outputs.

What I did in testbench:
- Applied different combinations of i0, i1, sel  
- Used `$dumpfile` and `$dumpvars`  
- Generated a `.vcd` file  

At first, I missed dumping signals properly, so waveform was empty. After fixing that, I got correct output.

---

## Step 3 – Simulation

Commands used:
iverilog good_mux.v tb_good_mux.v
vvp a.out

Here:
- `iverilog` compiles the code  
- `vvp` runs the simulation  

After running this, a `.vcd` file was generated.

This step is important because it checks whether the logic is working before moving to synthesis.

---

## Step 4 – Waveform Analysis

I opened the `.vcd` file in GTKWave.

In waveform:
- I checked how output changes with respect to select signal  
- Verified whether output matches expected MUX behavior  

This step gave more clarity than just seeing console output.

One thing I realized is that waveform helps in debugging much faster.

---

## Step 5 – Reading Design in Yosys

To start synthesis:
yosys

Inside Yosys:
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog good_mux.v

Here:
- Liberty file contains standard cell information  
- Verilog file is the design we wrote  

This step prepares the design for synthesis.

---

## Step 6 – Checking Design Statistics

After reading the design, I checked stats.

It shows:
- Number of wires  
- Number of cells  
- Structure of design  

This helped confirm that the design is properly read.

---

## Step 7 – Synthesis

Commands used:
synth
abc

During synthesis:
- RTL is converted into gate-level logic  
- Optimization happens automatically  
- Mapping to standard cells is done  

One important thing I observed is that synthesis is not just conversion, it also simplifies logic.

---

## Step 8 – Netlist Generation

After synthesis, a netlist is generated.

Netlist contains:
- Gates used (like AND, OR, NOT)  
- Connections between them  

This is closer to actual hardware representation compared to RTL.

---

## Step 9 – Netlist Visualization

Using Yosys `show` command, I visualized the design.

This gave a graphical view of how the circuit is implemented.

It was easier to understand compared to raw netlist.

---

## Observations

- Simulation and synthesis are completely different stages  
- Correct simulation is necessary before synthesis  
- RTL does not directly represent hardware  
- Synthesis introduces actual gate-level structure  

---

## What I Learned

- Basic RTL design flow from start to synthesis  
- Writing and verifying Verilog code  
- Importance of waveform analysis  
- How synthesis tools convert logic  

---

## Note

All the outputs and screenshots added in this folder are generated from my own system while performing the lab.