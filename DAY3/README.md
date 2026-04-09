# DAY 3 – Logic Optimization in RTL Design

---

## 1. Introduction to Logic Optimization

Logic optimization is the process of simplifying digital circuits to improve performance, reduce area, and minimize power consumption. During synthesis, redundant logic is removed and efficient implementations are generated without changing functionality.

---

## 2. Ternary Operator MUX

The ternary operator is a compact way to describe conditional logic in Verilog. It is commonly used to implement multiplexers.

### Verilog Code

```
module ternary_operator_mux (
    input a,
    input b,
    input sel,
    output y
);

assign y = sel ? a : b;

endmodule
```

### Explanation

* If sel = 1 → output y = a
* If sel = 0 → output y = b
* Synthesizer converts this into a 2:1 multiplexer

---

## 3. Yosys Synthesis Flow

### Commands Used

```
yosys
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty sky130_fd_sc_hd__tt_025C_1v80.lib
show
```

### Command Explanation

* read_liberty
  Loads standard cell library required for technology mapping

* read_verilog
  Reads the RTL design file

* synth -top
  Performs synthesis with specified top module

* abc -liberty
  Maps logic to standard cells

* show
  Displays synthesized circuit

---

## 4. Observations

* The ternary operator is synthesized into a multiplexer
* Yosys optimizes logic without changing functionality
* Mapping is done using SKY130 standard cell library

---

## 5. Screenshots

### RTL View

![RTL View](Screenshots/rtl_view.png)

### Synthesized Netlist

![Netlist](Screenshots/netlist.png)

### Yosys Output

![Yosys Output](Screenshots/yosys_output.png)

---

## 6. Key Learnings

* Learned how ternary operator works in RTL design
* Understood synthesis flow using Yosys
* Observed how RTL maps to gate-level implementation
* Importance of standard cell libraries in synthesis

---

## 7. Conclusion

Day 3 focused on logic optimization using the ternary operator. The design was synthesized successfully, and the results demonstrated how high-level RTL constructs are converted into optimized hardware using Yosys.

---
