# 📅 Day 5 – RTL Design Mastery

---

## 🚀 Overview

On Day 5, I explored different coding styles in Verilog and how they impact **simulation vs synthesis behavior**.
This includes understanding **complete/incomplete case statements**, **if conditions**, and **MUX/DEMUX implementations**.

---

## 📚 Topics Covered

1. Gate-Level Simulation (GLS)
2. Synthesis vs Simulation Mismatch
3. Blocking vs Non-Blocking (conceptual understanding)
4. Case statements (complete & incomplete)
5. If conditions (complete & incomplete)
6. MUX and DEMUX design
7. Ripple Carry Adder (RCA)

---

## ⚠️ Synthesis vs Simulation Mismatch

* Occurs when RTL simulation output differs from synthesized hardware
* Mainly caused due to:

  * Incomplete case statements
  * Missing default conditions
  * Improper sensitivity lists

---

## 🔍 Experiments & Observations

---

### 1️⃣ Bad Case Example

* Missing default condition
* Leads to latch inference

📸 Screenshots:

* ![Bad Case Code](screenshots/bad_case_code.png)
* ![Bad Case Simulation](screenshots/bad_case_simulation.png)

---

### 2️⃣ Complete Case

* Covers all input combinations
* No latch inferred

📸 Screenshots:

* ![Complete Case Code](screenshots/comp_case_code.png)
* ![Complete Case Simulation](screenshots/comp_case_simulation.png)
* ![Complete Case Synthesis](screenshots/comp_case_synthesis.png)

---

### 3️⃣ Incomplete Case

* Missing some input conditions
* Causes unintended latch

📸 Screenshots:

* ![Incomplete Case Code](screenshots/incomp_case_code.png)
* ![Incomplete Case Simulation](screenshots/incomp_case_simulation.png)
* ![Incomplete Case Logic](screenshots/incomp_case_logic.png)

---

### 4️⃣ Incomplete IF Conditions

* Missing else condition
* Leads to latch behavior

📸 Screenshots:

* ![Incomplete IF Code](screenshots/incomp_if_code.png)
* ![Incomplete IF Simulation](screenshots/incomp_if_simulation.png)

---

### 5️⃣ IF-ELSE Variations

📸 Screenshots:

* ![Incomplete IF2 Code](screenshots/incom_if2_code.png)
* ![Incomplete IF2 Simulation](screenshots/incomp_if2_simulation.png)
* ![Incomplete IF2 Synthesis](screenshots/incomp_if2_synthesis.png)

---

### 6️⃣ DEMUX Design

* Implemented using:

  * Case statements
  * Generate block

📸 Screenshots:

* ![DEMUX Code](screenshots/demux_code.png)
* ![DEMUX Simulation](screenshots/demux_simulation.png)
* ![DEMUX Synthesis](screenshots/demux_case_synthesis.png)
* ![DEMUX Generate Simulation](screenshots/demux_generate_simulation.png)
* ![DEMUX Generate Synthesis](screenshots/demux_generate_synthesis.png)

---

### 7️⃣ MUX Generator

📸 Screenshots:

* ![MUX Generator Code](screenshots/mux_generator_code.png)
* ![MUX Generator Simulation](screenshots/mux_generator_simulation.png)

---

### 8️⃣ Partial Case Assign

📸 Screenshots:

* ![Partial Case Logic](screenshots/partial_case_assign_logic.png)
* ![Partial Case Synthesis](screenshots/partial_case_assign_synthesis.png)

---

### 9️⃣ Ripple Carry Adder (RCA)

📸 Screenshots:

* ![RCA Code](screenshots/rca_code.png)
* ![RCA Simulation](screenshots/rca_simulation.png)

---

## 🧠 Key Learnings

* Always use **complete case statements**
* Add **default conditions** to avoid latches
* Incomplete IF or CASE → **Latch inference**
* Simulation may look correct but synthesis can differ
* Writing clean RTL ensures predictable hardware

---

## 🛠️ Tools Used

* VS Code
* Yosys (Synthesis)
* GTKWave (Simulation)

---

## 📌 Folder Structure

```
DAY5/
 ├── screenshots/
 └── README.md
```

---

## 🎯 Conclusion

Day 5 helped me understand how small mistakes in RTL coding can lead to major issues in hardware implementation.
Writing **synthesis-friendly code** is critical in VLSI design.

---
