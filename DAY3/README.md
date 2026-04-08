# DAY 3 – Logic Optimization in RTL Design

---

## 1. Introduction to Logic Optimization

Logic optimization is a critical step in digital design where redundant or inefficient logic is removed without altering the functional behavior of the circuit. The primary objective is to improve area, speed, and power efficiency.

Optimization is automatically performed by synthesis tools such as Yosys, but understanding how and why it happens is essential for writing efficient RTL.

---

## 2. Combinational Logic Optimization

### Explanation

Combinational logic optimization focuses on simplifying Boolean expressions and reducing unnecessary logic gates. The synthesis tool analyzes the logic and applies algebraic transformations to minimize gate count and logic depth.

![Combinational Optimization](screenshots/combinational_logic_optimisation.png)

---

### Engineering Insight

Reducing logic depth directly improves propagation delay. Fewer gates also result in reduced capacitance, which lowers both dynamic power consumption and switching delay.

---

## 3. Constant Propagation

### Explanation

Constant propagation is a technique where constant values (logic 0 or logic 1) are propagated through the circuit, allowing the synthesis tool to eliminate unnecessary logic.

![Constant Propagation](screenshots/constant_propagation.png)

---

### Example

```
Y = (A.B + C)'
If A = 0 → Y = (0 + C)' = C'
```

---

### Analysis

Since A is always 0, the AND operation becomes redundant. The logic simplifies to a single inverter. This reduces both gate count and delay.

---

### Engineering Insight

Constant propagation is one of the most powerful optimizations because it can completely remove logic blocks, not just simplify them.

---

## 4. Boolean Logic Optimization

### Explanation

Boolean optimization involves applying Boolean algebra to reduce complex expressions into minimal forms.

![Boolean Optimization](screenshots/boolean_optimisation.png)

---

### Example

```
y = a ? (b ? c : (c ? a : 0)) : (!c)
```

After simplification:

```
y = a ^ c
```

---

### Analysis

A nested multiplexer structure is reduced to a single XOR gate. This significantly reduces logic complexity and improves performance.

---

### Engineering Insight

Replacing multiplexers with simple gates reduces both area and delay. XOR-based implementations are often more efficient than deeply nested conditional logic.

---

## 5. Advanced Optimization Techniques

### Explanation

Advanced optimization techniques go beyond simple Boolean simplification and target structural improvements in the design.

![Advanced Optimization](screenshots/advanced_optimisation.png)

---

### Techniques

- State optimization removes unused or redundant states in sequential circuits  
- Cloning duplicates logic paths to reduce fanout delay  
- Retiming moves flip-flops across combinational logic to balance timing  

---

### Engineering Insight

These optimizations are critical in high-performance designs where timing closure is challenging.

---

## 6. Standard Synthesis Flow

```
read_liberty -lib lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog verilog_files/<file>.v
synth -top <module_name>
dfflibmap -liberty lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty lib/sky130_fd_sc_hd__tt_025C_1v80.lib
opt_clean -purge
show
```

---

### Flow Explanation

The RTL is first read into Yosys, followed by loading the timing library. Synthesis converts RTL into a generic netlist. The design is then optimized, mapped to standard cells, and cleaned to remove unused logic. Finally, the circuit structure can be visualized.

---

# LAB SECTION

---

## Objective

The objective of these labs is to analyze how different optimization techniques are applied during synthesis and how they affect the final hardware implementation. The focus is on observing how redundant logic, constants, and unnecessary registers are eliminated.

---

## 1. opt_check

### Code

```
assign y = a ? b : 0;
```

---

### Explanation

This code represents a multiplexer where the output is either `b` or `0` depending on the value of `a`.

---

### Optimization

```
y = a & b
```

---

### Output

![opt_check](screenshots/opt_check.png)

---

### Observations

The synthesis tool recognizes that selecting between `b` and `0` is equivalent to performing an AND operation between `a` and `b`. As a result, the multiplexer is completely removed and replaced with a simple AND gate. This reduces both hardware complexity and delay.

---

## 2. opt_check2

### Code

```
assign y = a ? 1 : b;
```

---

### Explanation

This represents a multiplexer selecting between constant `1` and input `b`.

---

### Optimization

```
y = a | b
```

---

### Output

![opt_check2](screenshots/opt_check2.png)

---

### Observations

The synthesis tool simplifies the logic by recognizing that choosing between `1` and `b` is equivalent to an OR operation. The multiplexer is eliminated and replaced with an OR gate, improving efficiency.

---

## 3. opt_check3

### Output

![opt_check3](screenshots/optcheck3_synth.png)

---

### Explanation

This lab demonstrates how complex conditional logic is simplified through Boolean optimization.

---

### Observations

The resulting hardware is significantly reduced compared to the original RTL. The synthesis tool applies multiple optimization passes to eliminate redundant conditions and minimize logic depth.

---

## 4. Boolean Logic Simplification

![Logic](screenshots/opt12_logic.png)

---

### Explanation

This experiment shows step-by-step reduction of a complex Boolean expression into a minimal equivalent form.

---

### Observations

The transformation highlights how intermediate logic terms are eliminated, resulting in a simpler and faster implementation.

---

## 5. dff_const1

### Synthesis Output

![Synthesis](screenshots/dff_constant_01_synth.png)

---

### Waveform

![Waveform](screenshots/dff_const1_waveform.png)

---

### Explanation

In this design, part of the flip-flop logic depends on constant values.

---

### Observations

The synthesis tool partially optimizes the flip-flop by removing unnecessary logic while retaining required sequential behavior. This demonstrates partial constant propagation in sequential circuits.

---

## 6. dff_const2

### Logic Output

![Logic](screenshots/dff_const2_logic.png)

---

### Waveform

![Waveform](screenshots/dff_const_waveform.png)

---

### Explanation

This case involves a flip-flop whose output is driven entirely by a constant value.

---

### Observations

The synthesis tool removes the flip-flop completely and directly ties the output to a constant. This is a strong example of sequential optimization where storage elements are eliminated.

---

## 7. dff_const3

### Logic Output

![Logic](screenshots/dff_const3_logic.png)

---

### Explanation

This design contains intermediate sequential elements with conditional behavior.

---

### Observations

The synthesis tool simplifies the structure while preserving required timing behavior. Some sequential elements are retained, but unnecessary logic is removed.

---

## 8. Counter Optimization

### Output

![Counter](screenshots/count_opt_synth.png)

---

### Explanation

This lab analyzes how counters are optimized when certain bits or conditions are unused.

---

### Observations

Unused bits are removed, and only essential counting logic is preserved. This reduces area and improves efficiency.

---

## 9. Counter Optimization 2

### Output

![Counter](screenshots/counter_opt2_synth.png)

---

### Explanation

Further optimization is applied to refine the counter logic.

---

### Observations

The synthesis tool aggressively trims redundant logic and keeps only the required functional components, resulting in a compact implementation.

---

## Flow of Execution

RTL → Optimization → Technology Mapping → Netlist Generation → Verification  

---

## Key Learnings

Constant propagation eliminates unnecessary logic by resolving constant signals early.

Boolean optimization reduces complex expressions into minimal gate-level implementations.

Sequential optimization removes redundant flip-flops and registers.

Synthesis tools automatically perform these optimizations, but writing clean RTL improves results significantly.

---

## Conclusion

This work demonstrates how synthesis transforms RTL into an optimized hardware implementation. It highlights that the written RTL is only an abstract description, and the final hardware is determined by optimization techniques applied during synthesis.

Understanding these optimizations is essential for designing efficient, high-performance digital systems.

---