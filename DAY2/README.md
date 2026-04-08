<h1>DAY 2 – Timing Libraries, PVT and Multi-Module Synthesis</h1>

<hr>

<h2>1. Timing Libraries (.lib)</h2>

<h3>Explanation</h3>
<p>
A timing library (.lib) is a technology-specific file that contains detailed electrical and timing characterization of standard cells. 
These libraries are generated after silicon characterization and are used by synthesis and timing tools to map RTL into real hardware.
</p>

<p>Each cell in the library contains:</p>
<ul>
<li>Propagation delay information for different input/output conditions</li>
<li>Transition (slew) data</li>
<li>Power consumption (dynamic and leakage)</li>
<li>Input capacitance and output load constraints</li>
<li>Setup and hold timing constraints for sequential elements</li>
</ul>

<p>
The delay values are stored as lookup tables dependent on input transition, output load capacitance, and PVT conditions. 
This makes synthesis timing-aware rather than purely logic-based.
</p>

<h3>Significance</h3>
<ul>
<li>Enables realistic hardware mapping</li>
<li>Ensures timing-aware synthesis</li>
<li>Allows accurate delay and power estimation</li>
<li>Forms the foundation for Static Timing Analysis (STA)</li>
</ul>

<h3>Reference</h3>
<ul>
<li><a href="https://skywater-pdk.readthedocs.io/en/latest/" target="_blank">SkyWater SKY130 PDK Documentation</a></li>
<li><a href="https://www.vlsi-expert.com/2012/03/delay-lookup-table-lib-file.html" target="_blank">Timing Library Explanation</a></li>
</ul>

<hr>

<h2>2. SKY130 Library Explanation</h2>

<p><b>Example:</b> sky130_fd_sc_hd__tt_025C_1v80.lib</p>

<ul>
<li>sky130 → 130nm technology node</li>
<li>fd → Foundry design</li>
<li>sc → Standard cell</li>
<li>hd → High density</li>
<li>tt → Typical process</li>
<li>025C → Temperature</li>
<li>1v80 → Voltage</li>
</ul>

<img src="screenshots/sky130_lib.png" width="600">

<h3>Reference</h3>
<ul>
<li><a href="https://skywater-pdk.readthedocs.io/en/latest/rules/library.html" target="_blank">SKY130 Library Details</a></li>
</ul>

<hr>

<h2>3. PVT (Process, Voltage, Temperature)</h2>

<h3>Explanation</h3>
<p>PVT represents real-world variations affecting circuit performance after fabrication.</p>

<ul>
<li><b>Process:</b> Fast (FF), Slow (SS)</li>
<li><b>Voltage:</b> High → faster, Low → slower</li>
<li><b>Temperature:</b> High → slow, Low → fast</li>
</ul>

<h3>Worst Case Condition</h3>
<p>Slow process, low voltage, high temperature</p>

<h3>Reference</h3>
<ul>
<li><a href="https://www.synopsys.com/glossary/what-is-pvt.html" target="_blank">PVT Explanation</a></li>
</ul>

<hr>

<h2>4. Delay, Power, Capacitance and Leakage</h2>

<h3>Delay</h3>
<p>Time taken for input change to reflect at output.</p>

<h3>Power</h3>
<p>P = α × C × V² × f</p>

<h3>Capacitance</h3>
<p>Higher capacitance increases delay.</p>

<h3>Reference</h3>
<ul>
<li><a href="https://www.vlsi-expert.com/2012/02/power-consumption-in-cmos.html" target="_blank">Power in CMOS</a></li>
</ul>

<hr>

<h2>5. Comparison of Same Modules</h2>

<p>
Different RTL implementations can produce the same functionality but different performance due to gate depth, logic levels, and load distribution.
</p>

<hr>

<h2>6. Stacked PMOS</h2>

<p>
Stacked PMOS refers to multiple PMOS transistors connected in series, leading to higher resistance and increased delay.
</p>

<hr>

<h2>7. Divide and Conquer</h2>

<p>
Large designs are divided into smaller modules for better readability, debugging, and synthesis optimization.
</p>

<hr>

<h2>LAB SECTION</h2>

<h3>Objective</h3>
<ul>
<li>Multi-module design</li>
<li>D Flip-Flop implementation</li>
<li>Synthesis and GLS</li>
</ul>

<hr>

<h2>1. Multiple Modules</h2>

<img src="screenshots/multiple_modules_code.png" width="600">
<img src="screenshots/multiple_modules_synthesis.png" width="600">
<img src="screenshots/multiple_modules_flat.png" width="600">
<img src="screenshots/multiple_modules_stats.png" width="600">

<hr>

<h2>2. DFF Async Reset</h2>

<img src="screenshots/code1.png" width="600">
<img src="screenshots/dff_asyncres_synthesis.png" width="600">
<img src="screenshots/dff_asyncres_simulation.png" width="600">

<hr>

<h2>3. DFF Async Set</h2>

<img src="screenshots/code2.png" width="600">
<img src="screenshots/dff_async_set_synthesis.png" width="600">
<img src="screenshots/async_set_simulation.png" width="600">

<hr>

<h2>4. DFF Sync Reset</h2>

<img src="screenshots/dff_syncres2.png" width="600">

<hr>

<h2>5. Multiply by 2</h2>

<img src="screenshots/mul2_code.png" width="600">
<img src="screenshots/mul2_synthesis.png" width="600">

<hr>

<h2>6. Multiply by 8</h2>

<img src="screenshots/mul8_code.png" width="600">
<img src="screenshots/mul8_synthesis.png" width="600">

<hr>

<h2>7. Flattened Design</h2>

<img src="screenshots/flat_multiple_mosules_synthesis.png" width="600">

<hr>

<h2>Commands</h2>

<pre>
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog multiple_modules.v
synth -top multiple_modules
dfflibmap -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
clean
write_verilog multiple_modules_net.v
</pre>

<pre>
iverilog my_lib/verilog_model/primitives.v 
my_lib/verilog_model/sky130_fd_sc_hd.v 
verilog_files/multiple_modules_net.v 
verilog_files/multiple_modules_tb.v

./a.out
gtkwave multiple_modules.vcd
</pre>

<hr>

<h2>Key Takeaways</h2>
<ul>
<li>Timing libraries enable realistic synthesis</li>
<li>PVT variations affect chip performance</li>
<li>Hierarchical design improves scalability</li>
<li>GLS validates synthesized design</li>
</ul>

<hr>

<h2>Conclusion</h2>
<p>
This section connects RTL design with real hardware behavior using timing libraries, synthesis, and simulation.
</p>