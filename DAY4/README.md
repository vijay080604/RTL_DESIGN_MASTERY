# DAY 4 – GLS, Blocking vs Non-Blocking, and MUX Design

---

## 1. Introduction

In this lab, I explored important RTL design concepts including Gate Level Simulation (GLS), blocking vs non-blocking assignments, and different ways of designing multiplexers. The focus was to understand how coding style affects synthesized hardware and simulation behavior.

---

## 2. Gate Level Simulation (GLS)

GLS is performed after synthesis to verify that the synthesized netlist behaves the same as the RTL design. It ensures that there are no mismatches between expected and actual hardware behavior.

GLS uses:
- Synthesized netlist  
- Standard cell libraries  
- Same testbench  

This step is critical for validating real hardware functionality.

---

## 3. Ternary Operator MUX

### RTL Code

![Ternary Operator Code](DAY4/screenshots/ternary_operator_mux_code.png)

### Synthesis Output

![Ternary Operator Synthesis](DAY4/screenshots/ternary_operator_mux_synthesis.png)

### Simulation Output

![Ternary Operator Simulation](DAY4/screenshots/ternary_operator_simulation.png)

### GLS Output

![Ternary Operator GLS](DAY4/screenshots/ternary_operator_gls.png)

### Explanation

The ternary operator is the most efficient way to design a multiplexer in Verilog. The synthesis tool directly maps this expression into a standard MUX cell from the SKY130 library. The simulation waveform shows correct switching behavior based on the select signal. During GLS, the waveform matches RTL perfectly, proving functional equivalence. This confirms that concise RTL leads to optimized and predictable hardware implementation.

---

## 4. Bad MUX (Incorrect Coding Style)

### RTL Code

![Bad MUX Code](DAY4/screenshots/bad_mux_code.png)

### Simulation Output

![Bad MUX Simulation](DAY4/screenshots/bad_mux_simulation.png)

### GLS Output

![Bad MUX GLS](DAY4/screenshots/bad_mux_gls.png)

### Explanation

In this design, the MUX is written using an always block with sensitivity only to the select signal. This is incorrect because the output should also depend on input signals. Due to this incomplete sensitivity list, simulation may produce incorrect or misleading results. GLS reveals the actual hardware behavior, which may differ from RTL simulation. This demonstrates why proper sensitivity lists or continuous assignments should be used.

---

## 5. Blocking Assignment Caveat

### RTL Code

![Blocking Caveat Code](DAY4/screenshots/blocking_caveat_code.png)

### Simulation Output

![Blocking Caveat Simulation](DAY4/screenshots/blocking_caveat_simulation.png)

### GLS Output

![Blocking Caveat GLS](DAY4/screenshots/blocking_caveat_gls.png)

### Explanation

Blocking assignments execute sequentially within an always block, which can lead to unintended behavior in combinational logic. In this example, intermediate variables affect final outputs in a way that may not reflect parallel hardware execution. The simulation waveform shows dependency issues, while GLS reveals how synthesis interprets the logic. This highlights the importance of using non-blocking assignments or proper coding styles for predictable hardware behavior.

---

## 6. Key Learnings

- GLS is essential to verify synthesized hardware  
- Ternary operator produces clean and optimized MUX design  
- Poor coding styles lead to mismatches between RTL and GLS  
- Blocking assignments can cause unintended logic behavior  
- Writing proper RTL ensures correct synthesis and simulation  

---

## 7. Conclusion

Day 4 provided a deeper understanding of how RTL design choices impact real hardware. By comparing RTL simulation with GLS, I learned how synthesis tools interpret code and optimize logic. Writing clean, correct, and hardware-aware Verilog is crucial for successful VLSI design.

---