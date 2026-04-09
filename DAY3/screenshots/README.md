# DAY 3 – Logic Optimization in RTL Design

---

## 1. Introduction

In this lab, I explored how logic optimization is performed during synthesis using Yosys. The aim was to understand how unnecessary logic, constant values, and redundant conditions are identified and removed. This process is important because it directly impacts the efficiency of the final hardware in terms of area, speed, and power.

---

## 2. Logic Optimization

Logic optimization simplifies a digital circuit without affecting its functionality. During synthesis, tools like Yosys analyze the RTL code and eliminate redundant logic, simplify expressions, and remove unused signals.

Key benefits observed:

* Reduced number of gates
* Better hardware efficiency
* Simplified circuit structure
* Improved performance

---

## LAB EXPERIMENTS

---

## 1. opt_check

### Code

![Code](DAY3/screenshots/opt_check_code.png)

### Explanation

This design uses a ternary operator to describe conditional logic. Depending on the value of input signals, the output is selected between different expressions. Although the RTL appears complex, synthesis tools interpret this as multiplexing logic and simplify it. This example helps in understanding how high-level conditional statements are internally reduced to basic logic gates.

---

### Simulation

![Simulation](DAY3/screenshots/opt_check_simulation.png)

### Explanation

The waveform confirms that the output follows the expected logic for all input combinations. As the inputs change over time, the output transitions correctly according to the condition defined in the RTL. This step ensures that the design is functionally correct before moving to synthesis, which is important because optimization should not alter behavior.

---

### Synthesis

![Synthesis](DAY3/screenshots/opt_check_synthesis.png)

### Explanation

After synthesis, the complex ternary logic is reduced to a minimal gate-level implementation. Redundant logic paths are removed, and only essential gates remain. This shows how synthesis tools convert high-level conditions into efficient hardware using basic standard cells.

---

## 2. opt_check2

### Code

![Code](DAY3/screenshots/opt_check2_code.png)

### Explanation

This design represents conditional logic involving multiple inputs. Some of the conditions may overlap or simplify logically. The purpose of this example is to observe how synthesis tools detect such redundancy and optimize the circuit.

---

### Simulation

![Simulation](DAY3/screenshots/opt_check2_simulation.png)

### Explanation

The simulation waveform shows correct behavior for all input combinations. Each transition of the inputs produces the expected output. This confirms that the RTL design is valid and provides a reference to compare with the optimized design after synthesis.

---

### Synthesis

![Synthesis](DAY3/screenshots/opt_check2_synthesis.png)

### Explanation

The synthesis result shows a simplified implementation, often reduced to a single OR or equivalent gate. Yosys removes redundant conditions and unnecessary intermediate signals. This demonstrates how even slightly complex RTL logic can be reduced to very simple hardware.

---

## 3. opt_check3

### Code

![Code](DAY3/screenshots/opt_check3_code.png)

### Explanation

This design contains nested ternary conditions, which introduce multiple decision levels in RTL. Such nested structures can often be simplified logically. This example helps in understanding how synthesis tools flatten nested conditions and produce a compact circuit.

---

### Simulation

![Simulation](DAY3/screenshots/opt_check3_simulation.png)

### Explanation

The waveform shows that the output changes correctly based on input variations. Even though the RTL logic is nested, the output behaves correctly in all cases. This confirms that the design is functionally accurate before optimization.

---

### Synthesis

![Synthesis](DAY3/screenshots/opt_check3_synthesis.png)

### Explanation

The synthesized circuit is much simpler compared to the RTL description. The nested conditions are reduced to a basic AND or equivalent gate. This indicates that Yosys has removed unnecessary logic levels and optimized the design efficiently.

---

## 4. opt_check4

### Code

![Code](DAY3/screenshots/opt_check4_code.png)

### Explanation

This design includes multiple nested conditional expressions with three inputs. The RTL looks complex, but many of these conditions are logically equivalent or redundant. This example is useful to observe how synthesis tools handle complex expressions.

---

### Synthesis

![Synthesis](DAY3/screenshots/opt_check4_synthesis.png)

### Explanation

The synthesis output shows that the complex expression has been reduced to a simple XNOR-based implementation. This proves that synthesis tools can identify logical equivalence and significantly reduce circuit complexity.

---

## 5. dff_const1

### Code

![Code](DAY3/screenshots/dff_const1_code.png)

### Explanation

This design connects a flip-flop to a constant input. Since the input does not change, the flip-flop does not perform meaningful sequential operation. This example demonstrates how constant values affect sequential logic during synthesis.

---

### Simulation

![Simulation](DAY3/screenshots/dff_const1_simulation.png)

### Explanation

The waveform shows that the output remains constant over time. Since the input is fixed, there is no change in output with clock cycles. This confirms correct functionality and helps in understanding constant-driven sequential behavior.

---

### Synthesis

![Synthesis](DAY3/screenshots/dff_const1_synthesis.png)

### Explanation

During synthesis, the flip-flop is removed and replaced with a constant assignment. This reduces hardware usage by eliminating unnecessary sequential elements.

---

## 6. dff_const2

### Simulation

![Simulation](DAY3/screenshots/dff_const2_simulation.png)

### Explanation

The waveform confirms that the output is constant for all time intervals. This indicates that the flip-flop input does not vary, making the sequential element redundant.

---

### Synthesis

![Synthesis](DAY3/screenshots/dff_const2_synthesis.png)

### Explanation

The synthesis tool removes the flip-flop completely and directly assigns a constant value to the output. This is an example of optimization in sequential logic.

---

## 7. Multiple Modules Optimization

### Synthesis

![Synthesis](DAY3/screenshots/multiple_modules_opt_synthesis.png)

### Explanation

This example involves multiple modules. During synthesis, Yosys analyzes the full hierarchy and removes unused or redundant modules. The final design retains only necessary logic.

---

### Synthesis 2

![Synthesis](DAY3/screenshots/multiple_modules_opt2_synthesis.png)

### Explanation

Further optimization reduces inter-module redundancy. The final circuit is compact and efficient, showing the effectiveness of hierarchical optimization.

---

## 8. Counter Optimization

### Code

![Code](DAY3/screenshots/counter_opt_code.png)

### Explanation

This design implements a counter with additional logic. Some of this logic may not be necessary depending on usage. This experiment helps in understanding optimization in sequential circuits.

---

### Simulation

![Simulation](DAY3/screenshots/counter_opt_simulation.png)

### Explanation

The waveform shows correct counting behavior. The counter increments as expected, confirming that the design is functionally correct.

---

### Synthesis 1

![Synthesis](DAY3/screenshots/counter_opt1_synthesis.png)

### Explanation

The initial synthesis output shows partial optimization. Some redundant logic is removed while maintaining counter functionality.

---

### Synthesis 2

![Synthesis](DAY3/screenshots/counter_opt2_synthesis.png)

### Explanation

The final synthesis output shows a fully optimized counter with fewer gates and simplified control logic. This highlights the impact of optimization on sequential designs.

---

## Conclusion

In this lab, I learned how synthesis tools optimize both combinational and sequential circuits. Redundant logic, constant-driven elements, and unused modules are removed to create efficient hardware. This lab provided a clear understanding of how RTL is transformed into optimized gate-level implementation.
