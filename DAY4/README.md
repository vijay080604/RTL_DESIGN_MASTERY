# 📅 Day 4 – RTL Design & Synthesis Workshop

---

## 🚀 Overview

Welcome to **Day 4** of the RTL Design & Synthesis Workshop.

Today’s session focuses on **core RTL design concepts that directly impact real silicon behavior**:

- Gate-Level Simulation (GLS)
- Blocking vs Non-Blocking Assignments
- Synthesis vs Simulation Mismatch

📌 These are **very important for VLSI interviews + industry design practices**

---

## 📚 Table of Contents

1. Gate-Level Simulation (GLS)  
2. Synthesis vs Simulation Mismatch  
3. Blocking vs Non-Blocking  
4. Labs (with Commands)  
5. Key Learnings  

---

# 🧠 1. Gate-Level Simulation (GLS)

## 📌 Definition

GLS is the process of simulating the **gate-level netlist generated after synthesis**.

👉 Flow:  
RTL → Synthesis → Gate-Level Netlist → GLS

---

## 🎯 Why GLS is Important?

- ✅ Ensures synthesis tool correctly converted RTL to gates  
- ✅ Detects **timing-related issues** (setup/hold violations)  
- ✅ Verifies **real hardware behavior**  
- ✅ Helps validate **DFT structures (scan chains)**  

---

## 🔄 GLS Flow

![GLS Flow](screenshots/gls_flow.png)

---

## ⚙️ Types of GLS

1. **Functional GLS**
   - No delays (ideal simulation)
   - Used for logic verification

2. **Timing GLS**
   - Includes delays (SDF annotation)
   - Used for timing validation

---

## ⚙️ Commands Used

### Step 1: Compile
```bash
iverilog primitives.v sky130_fd_sc_hd.v design.v tb.v
```

### Step 2: Run Simulation
```bash
./a.out
```

### Step 3: View Waveform
```bash
gtkwave dump.vcd
```

---

# ⚠️ 2. Synthesis vs Simulation Mismatch

## 📌 Definition

Mismatch occurs when:

👉 RTL Simulation Output ≠ Gate-Level Output

---

## ❗ Why It Happens?

1. **Missing Sensitivity List**
2. **Wrong assignment type (= vs <=)**
3. **Improper coding style**
4. **Use of non-synthesizable constructs**
5. **Blocking order issues**

---

## 🔥 Real Example

![Mismatch](screenshots/wrong_bad_mux.png)

👉 Simulator assumes ideal behavior  
👉 Synthesis creates real hardware → mismatch occurs

---

# 🔁 3. Blocking vs Non-Blocking

---

## 🔵 Blocking (=)

- Executes **sequentially (top to bottom)**
- Immediate update
- Order of statements matters
- Used in **combinational logic**

```verilog
always @(*) begin
  a = b;
  c = a;  // uses updated 'a'
end
```

📌 Important:
👉 Each line executes immediately → like software execution

---

## 🟢 Non-Blocking (<=)

- Executes **in parallel**
- Updates happen at end of time step
- Used in **sequential logic (flip-flops)**

```verilog
always @(posedge clk) begin
  q <= d;
end
```

📌 Important:
👉 All RHS evaluated first → then assigned together

---

## 📸 Comparison

![Comparison](screenshots/blocking_vs_nonblocking.png)

---

# ⚠️ 4. Missing Sensitivity List

## ❌ Wrong Code

```verilog
always @(sel)
```

👉 Only triggers when `sel` changes  
👉 Ignores `i0`, `i1` → incorrect simulation  

---

## ✅ Correct Code

```verilog
always @(*)
```

👉 Automatically includes all inputs  

---

## 📸 Visualization

![Sensitivity](screenshots/missing_sentivity.png)

---

# 🧪 5. LABS (WITH COMMANDS)

---

## 🔬 Lab 1 – Ternary Operator MUX

```verilog
module ternary_operator_mux (input i0, input i1, input sel, output y);
  assign y = sel ? i1 : i0;
endmodule
```

---

### ▶️ Simulation Commands

```bash
iverilog ternary_operator_mux.v tb.v
./a.out
gtkwave dump.vcd
```

---

## 📸 Output

![MUX Output](screenshots/correct_ternary_operator.png)

---

## 🔬 Lab 2 – Synthesis using Yosys

### ▶️ Commands

```bash
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

---

## 📸 Output

![Synthesis](screenshots/ternary_synthesis.png)

---

## 🔬 Lab 3 – GLS of MUX

### ▶️ Commands

```bash
iverilog primitives.v sky130_fd_sc_hd.v ternary_operator_mux.v tb.v
./a.out
gtkwave dump.vcd
```

---

## 📸 Output

![GLS](screenshots/gls_bad_mux.png)

---

## 🔬 Lab 4 – Bad MUX

```verilog
module bad_mux (input i0, input i1, input sel, output reg y);
  always @(sel) begin
    if(sel)
      y <= i1;
    else
      y <= i0;
  end
endmodule
```

---

## ❌ Issues

- Missing sensitivity list  
- Using non-blocking in combinational logic  
- Causes mismatch  

---

## 📸 Output

![Bad MUX](screenshots/bad_mux_comparition.png)

---

## 🔬 Lab 5 – Fixed MUX

```verilog
always @(*) begin
  if(sel)
    y = i1;
  else
    y = i0;
end
```

---

## 📸 Correct Output

![Fixed](screenshots/correct_blocling_cavity.png)

---

## 🔬 Lab 6 – Blocking Caveat

```verilog
module blocking_caveat(input a, input b, input c, output reg d);
  reg x;
  always @(*) begin
    d = x & c;
    x = a | b;
  end
endmodule
```

---

## ❗ Problem

👉 Order matters → uses **old value of x**

---

## 📸 Issue

![Issue](screenshots/blocking_caveat.png)

---

## ✅ Correct Code

```verilog
always @(*) begin
  x = a | b;
  d = x & c;
end
```

---

## 📸 Correct Output

![Correct](screenshots/right_blocking_cavity.png)

---

## 🔬 Lab 7 – Synthesis

### ▶️ Commands

```bash
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog blocking_caveat.v
synth -top blocking_caveat
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

---

## 📸 Output

![Synthesis](screenshots/synth_blocking_cavity.png)

---

# 🔥 Advanced Understanding

![Detailed](screenshots/blocking_cavity_comparision.png)

👉 Demonstrates real hardware difference due to assignment order

---

# 🧠 Key Learnings

✔ Use `always @(*)` for combinational logic  
✔ Use `=` → combinational  
✔ Use `<=` → sequential  
✔ Order matters in blocking assignments  
✔ Always verify using GLS  
✔ Avoid synthesis-simulation mismatch  

---

# 💡 Final Tip

> 🔥 “RTL working ≠ Hardware working”

Always verify:
- RTL Simulation  
- Gate-Level Simulation  