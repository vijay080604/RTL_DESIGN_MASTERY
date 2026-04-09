# DAY 3 – Logic Optimization and Sequential Optimizations in RTL Design

---

## 1. Introduction

Day 3 focuses on analyzing how synthesis tools optimize RTL designs at both combinational and sequential levels. Using Yosys, various designs were synthesized to observe how redundant logic, constant-driven signals, and unused elements are removed.

The objective is to understand how RTL descriptions are transformed into efficient gate-level implementations while preserving functional correctness.

---

## 2. Counter Optimization

### Description

A counter design was implemented and synthesized to study how optimization techniques reduce unnecessary logic and improve circuit efficiency.

---

### Screenshots

#### Code

![Counter Code](screenshots/counter_opt_code.png)

#### Simulation

![Counter Simulation](screenshots/counter_opt_simulation.png)

#### Synthesis 1

![Counter Synth 1](screenshots/counter_opt1_synthesis.png)

#### Synthesis 2

![Counter Synth 2](screenshots/counter_opt2_synthesis.png)

---

### Detailed Analysis

* In the RTL simulation, the counter increments sequentially based on clock transitions, confirming correct functional behavior.

* During synthesis, Yosys analyzes the logic and identifies redundant operations such as unused bits, constant conditions, or logic that does not affect the final output.

* In the first synthesis output:

  * The counter is mapped directly to flip-flops and combinational increment logic.
  * Basic optimization is applied but retains most structural elements.

* In the second synthesis output:

  * Advanced optimizations remove unnecessary intermediate signals.
  * Logic is simplified using constant propagation and Boolean minimization.
  * Gate count is reduced, improving area efficiency.

* The optimized netlist shows fewer cells and simplified interconnections compared to the initial mapping.

---

## 3. D Flip-Flop Constant Optimization

### Description

Sequential circuits with constant inputs were analyzed to understand how synthesis tools eliminate unnecessary storage elements and simplify logic.

---

### Case 1

#### Code

![DFF Const1 Code](screenshots/dff_const1_code.png)

#### Simulation

![DFF Const1 Simulation](screenshots/dff_const1_simulation.png)

#### Synthesis

![DFF Const1 Synthesis](screenshots/dff_const1_synthesis.png)

---

### Detailed Analysis

* Simulation shows that the flip-flop captures input data based on clock edges.

* When the input to the flip-flop is driven by a constant value:

  * The output becomes predictable and does not depend on dynamic input signals.

* During synthesis:

  * Yosys identifies that the flip-flop does not store meaningful changing data.
  * The flip-flop is either removed or replaced with a constant driver.

* This is an example of **constant propagation and sequential optimization**, where unnecessary memory elements are eliminated.

---

### Case 2

#### Simulation

![DFF Const2 Simulation](screenshots/dff_const2_simulation.png)

#### Synthesis

![DFF Const2 Synthesis](screenshots/dff_const2_synthesis.png)

---

### Detailed Analysis

* Similar to Case 1, the presence of constant inputs leads to predictable outputs.

* The synthesis tool:

  * Removes redundant flip-flops
  * Directly connects outputs to constant logic levels

* This reduces both area and power consumption since clock-driven elements are minimized.

---

## 4. Multiple Modules Optimization

### Description

Designs consisting of multiple interconnected modules were synthesized to observe optimization across module boundaries.

---

### Screenshots

#### Synthesis 1

![Multiple Modules Synth 1](screenshots/multiple_modules_opt_synthesis.png)

#### Synthesis 2

![Multiple Modules Synth 2](screenshots/multiple_modules_opt2_synthesis.png)

---

### Detailed Analysis

* In hierarchical designs, each module is initially synthesized separately.

* During optimization:

  * Yosys may flatten the hierarchy to analyze the entire design globally.
  * Redundant logic between modules is identified and removed.

* Observations from outputs:

  * Duplicate logic across modules is merged
  * Intermediate wires are eliminated
  * Overall gate count is reduced

* Flattened synthesis provides better optimization at the cost of losing module boundaries.

---

## 5. Optimization Check Cases

### Description

Multiple test cases were designed to validate how synthesis tools handle different optimization scenarios such as redundant logic, constant conditions, and unused signals.

---

### Case 1

#### Code

![Opt Check Code](screenshots/opt_check_code.png)

#### Simulation

![Opt Check Simulation](screenshots/opt_check_simulation.png)

#### Synthesis

![Opt Check Synthesis](screenshots/opt_check_synthesis.png)

---

### Analysis

* Simulation confirms expected logical behavior.

* During synthesis:

  * Redundant conditional statements are simplified
  * Equivalent logic expressions are merged
  * Unused signals are removed

---

### Case 2

#### Code

![Opt Check2 Code](screenshots/opt_check2_code.png)

#### Simulation

![Opt Check2 Simulation](screenshots/opt_check2_simulation.png)

#### Synthesis

![Opt Check2 Synthesis](screenshots/opt_check2_synthesis.png)

---

### Analysis

* Logic with constant branches is reduced to a single effective path.

* Multiplexers with constant select signals are simplified into direct connections.

---

### Case 3

#### Code

![Opt Check3 Code](screenshots/opt_check3_code.png)

#### Simulation

![Opt Check3 Simulation](screenshots/opt_check3_simulation.png)

#### Synthesis

![Opt Check3 Synthesis](screenshots/opt_check3_synthesis.png)

---

### Analysis

* Boolean simplification reduces complex expressions.

* Logic gates are minimized using algebraic optimization techniques.

---

### Case 4

#### Code

![Opt Check4 Code](screenshots/opt_check4_code.png)

#### Synthesis

![Opt Check4 Synthesis](screenshots/opt_check4_synthesis.png)

---

### Analysis

* Dead code (logic not affecting outputs) is completely removed.

* Final netlist contains only essential logic required for functionality.

---

## 6. Key Learnings

* Understood how synthesis tools perform constant propagation and dead code elimination
* Learned how redundant flip-flops and logic are removed in sequential optimization
* Observed Boolean simplification and logic minimization techniques
* Gained insight into hierarchical vs flattened optimization
* Learned how RTL is transformed into an efficient gate-level netlist

---

## 7. Conclusion

Day 3 provided a detailed understanding of logic optimization in RTL design. By analyzing multiple case studies, it was observed how Yosys improves design efficiency by reducing gate count, eliminating redundant logic, and simplifying sequential elements while maintaining functional correctness.

---
