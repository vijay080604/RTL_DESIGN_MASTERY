# DAY 5 – Generate Blocks, Incomplete Cases, and Arithmetic Circuits

---

## 1. Introduction

In this lab, I explored advanced RTL design concepts including generate statements, incomplete case/if conditions, and arithmetic circuit design using ripple carry adders (RCA). The focus was to understand how scalable hardware is designed and how improper coding can lead to unintended latch inference during synthesis.

---

## 2. DEMUX using Case Statement

### Simulation Output
![DEMUX Case Simulation](DAY5/screenshots/demux_case_simulation.png)

### Synthesis Output
![DEMUX Case Synthesis](DAY5/screenshots/demux_case_synthesis.png)

### Explanation

The DEMUX is implemented using a case statement where the input is routed to one of the outputs based on the select signal. The simulation waveform shows correct one-hot behavior, where only one output is active at a time. During synthesis, the tool converts this into combinational logic using AND/OR gates. Since all cases are covered, no latch is inferred, ensuring proper combinational implementation.

---

## 3. DEMUX using Generate Statement

### RTL Code
![DEMUX Generate Code](DAY5/screenshots/demux_generate_code.png)

### Simulation Output
![DEMUX Generate Simulation](DAY5/screenshots/demux_generate_simulation.png)

### Synthesis Output
![DEMUX Generate Synthesis](DAY5/screenshots/demux_generate_synthesis.png)

### GLS Output
![DEMUX Generate GLS](DAY5/screenshots/demux_generator_gls.png)

### Explanation

The generate construct is used to create scalable hardware using loops. In this DEMUX design, multiple outputs are generated using a for-loop, making the design compact and reusable. The simulation confirms correct behavior for all select values. Synthesis expands the loop into parallel hardware. GLS matches RTL output, proving that generate blocks correctly translate into actual hardware structures.

---

## 4. Incomplete Case Statement

### RTL Code
![Incomplete Case Code](DAY5/screenshots/incomp_codes.png)

### Simulation Output
![Incomplete Case Simulation](DAY5/screenshots/incomp_case_simulation.png)

### Synthesis Output
![Incomplete Case Synthesis](DAY5/screenshots/incomp_case_synthesis.png)

### Explanation

In this example, not all possible input conditions are covered in the case statement. As a result, the output retains its previous value for unspecified cases. This behavior forces the synthesis tool to infer a latch. The synthesized diagram clearly shows latch elements, highlighting unintended sequential behavior. This demonstrates why all cases must be explicitly defined in combinational logic.

---

## 5. Incomplete IF Statements

### IF Condition Simulation
![Incomplete IF Simulation](DAY5/screenshots/incomp_if_simulation.png)

### IF Synthesis Output
![Incomplete IF Synthesis](DAY5/screenshots/incomp_if_synthesis.png)

### IF-ELSE Condition Simulation
![Incomplete IF2 Simulation](DAY5/screenshots/incomp_if2_simulation.png)

### Explanation

Incomplete if statements also lead to latch inference when outputs are not assigned in all branches. In the first case, missing else conditions cause the output to hold previous values. In the second case, partial conditions still result in unintended storage elements. The synthesis diagrams confirm latch inference. This highlights the importance of writing fully specified combinational logic.

---

## 6. MUX using Generate

### Simulation Output
![MUX Generate Simulation](DAY5/screenshots/mux_generate_simulation.png)

### Synthesis Output
![MUX Generate Synthesis](DAY5/screenshots/mux_generate_synthesis.png)

### GLS Output
![MUX Generate GLS](DAY5/screenshots/mux_generate_gls.png)

### Explanation

The MUX is implemented using generate constructs to handle multiple inputs efficiently. The simulation waveform verifies correct selection based on control signals. During synthesis, the design is mapped into optimized multiplexer cells. GLS confirms functional equivalence with RTL. This demonstrates how generate blocks simplify large designs while maintaining correctness.

---

## 7. Ripple Carry Adder (RCA)

### RTL Code
![RCA Code](DAY5/screenshots/rca_fa_code.png)

### Simulation Output
![RCA Simulation](DAY5/screenshots/rca_simulation.png)

### GLS Output
![RCA GLS](DAY5/screenshots/rca_gls.png)

### Explanation

The ripple carry adder is built using multiple full adders connected in series using a generate loop. Each stage passes the carry to the next stage, creating a chain structure. The simulation waveform shows correct addition of two 8-bit numbers. GLS results match RTL, confirming proper synthesis. This example demonstrates hierarchical design and scalability using generate constructs.

---

## 8. Key Learnings

- Generate blocks help design scalable and reusable hardware  
- Case and if statements must be complete to avoid latch inference  
- Latches are unintentionally created due to incomplete assignments  
- GLS helps verify synthesized hardware behavior  
- Complex designs like RCA can be efficiently built using modular design  

---

## 9. Conclusion

Day 5 focused on writing scalable and correct RTL code using generate constructs and understanding the risks of incomplete logic descriptions. By analyzing synthesis results and GLS, I learned how improper coding leads to unintended hardware such as latches. This lab strengthened my understanding of writing clean and hardware-efficient Verilog.

---