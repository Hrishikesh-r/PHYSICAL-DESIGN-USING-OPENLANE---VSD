#  Day - 4 Timing Analysis and Clock Tree Synthesis (CTS)

Standard Cell LEF Generation Process
A Library Exchange Format (LEF) file contains only the necessary details of a standard cell for the physical design process, such as PR boundaries, I/O ports, and power/ground rails. Below are the steps to generate a LEF file from a .mag file:

1. Grid into Track Information

Tracks are used for routing metal layers and are essential for determining the height and width of the standard cell.

Track Guidelines:

I/O Ports: Must lie on the intersection of horizontal and vertical tracks.

Width/Height of the Standard Cell: Should be odd multiples of horizontal and vertical track pitches.

Tracks Info Example (tracks.info):

li1 X 0.23 0.46  
li1 Y 0.17 0.34 

Setting the Grid in TkCon:

grid 0.46um 0.34um 0.23um 0.17um  

2. Port Definition in Magic

Ports define the input, output, power, and ground connections of a cell and must be correctly set up for LEF extraction.

Steps to Define Ports in Magic:

Load the .mag File:

Open the layout of the design (e.g., inverter) in Magic.

Navigate to the Magic Layout Window.

Create Port Labels:

Go to Edit >> Text to open the text dialogue box.

Use the S key (double press) on I/O labels to automatically set the string name and size.

Ensure the Port Enable checkbox is checked and Default is unchecked.

Set Port Properties in TkCon:

For Input Port (A):

port class input  

port use signal

For Output Port (Y):

port class output  

port use signal

For Power Rail (VPWR):

port class inout  

port use power

For Ground Rail (VGND):

port class inout  

port use ground

Verify Port Declaration:

After setting the ports, reload the .mag file.

Properly declared ports will flash/change color, indicating correct setup.

3. Extract LEF File from Magic

Once the ports and tracks are properly defined:

Open the TkCon window.

Use the following command to extract the LEF file:

lef write <filename>.lef

This will generate the LEF file containing the necessary placement and routing details for the standard cell.

By following these steps, the generated LEF file will ensure accurate integration of the custom standard cell into the overall design flow.

![image](https://github.com/user-attachments/assets/0658d1d2-8ab2-4ac3-aedc-dd1f073e2fd5)

### Steps to include custom cell in ASIC design

Modify the config.tcl as follows to include the custom cell in the physical design flow :

![image](https://github.com/user-attachments/assets/31a94795-b99c-404f-a878-8e82f4249fb8)

Then follow the next set of commands to run synthesis until floorplan: 

set lefs [glob $::env(DESIGN_DIR)/src/*.lef]

add_lefs -src $lefs

run_synthesis

run_floorplan

Note: if the floorplan fails during this stage then try the below commands :

/*

init_floorplan

place_io

tap_decap_or

*/

### Delay Tables
Delay tables are critical timing models used in digital design to characterize and optimize the timing behavior of standard cells, particularly during Static Timing Analysis (STA) and Clock Tree Synthesis (CTS). These tables provide numerical data representing the propagation delay and output slew of a cell under different input and output conditions. The key parameters used in delay tables are:

Input Slew: This is the rate at which the input voltage changes over time. It is typically a function of the driving cell's output characteristics and the connected load.

Output Load Capacitance: The capacitive load at the output of a cell influences its timing. Larger loads lead to increased delay and output slew.

Propagation Delay: The time taken for a signal to travel from the 50% voltage level of the input to the 50% voltage level of the output.

Output Slew: The rate of voltage change at the output, which can affect downstream cells' timing behavior.

Structure of Delay Tables

Delay tables are typically two-dimensional matrices where rows represent different input slews, and columns correspond to various output load capacitances. Each cell in the matrix contains the propagation delay or output slew value for the corresponding input and output conditions.

Significance in Clock Tree Synthesis (CTS)

In CTS, delay tables are essential for balancing clock paths to ensure minimal skew, which is the difference in clock arrival times at different parts of the circuit. A well-balanced clock tree ensures synchronous operation and avoids timing violations. Skew can arise due to variations in buffer sizes, capacitive loads, or wire lengths. By referencing delay tables, designers can size buffers and balance loads to equalize delay across all clock tree paths.

Usage in Timing Analysis

During STA, delay tables help evaluate the worst-case timing scenarios, considering variations in:

Process: Variations in manufacturing can affect transistor performance.

Voltage: Supply voltage fluctuations impact delay and slew.

Temperature: Temperature variations alter transistor speed and resistance.

By considering these factors, delay tables ensure accurate modeling and adherence to setup and hold timing constraints.

Optimization Using Delay Tables

Delay tables are instrumental in various optimization processes:

Cell Sizing: Adjusting cell sizes to balance timing and power.

Buffer Insertion: Adding buffers to drive high fan-out nets and reduce wire delay.

Clock Skew Optimization: Ensuring that clock signals arrive simultaneously across all registers.

![image](https://github.com/user-attachments/assets/a5be4ce4-86bf-419a-8158-6d4785343e99)

### Post-synthesis timing analysis

Post-synthesis timing analysis ensures that a synthesized design meets timing constraints before moving to subsequent stages of the physical design flow. OpenSTA, a robust static timing analysis (STA) tool, performs this analysis outside the OpenLANE flow. It requires a configuration file (pre_sta.conf) to define the environment, constraints, and design inputs. Timing analysis is invoked with:

sta pre_sta.conf

Key Elements of Post-Synthesis Timing Analysis

Clock Propagation and Ideal Clock Assumptions:

Before Clock Tree Synthesis (CTS), the clock signal is treated as ideal. This assumption simplifies the analysis by assuming zero clock skew, meaning the clock arrives at all endpoints simultaneously.
Timing checks are limited to setup analysis, as hold timing checks require an accurate model of the clock distribution network, which is created during CTS.

Setup Timing and Slack:

Setup Time: The minimum time required for data to be stable before the active clock edge to ensure proper latching.

Setup Slack: Indicates the margin between the data arrival time and the required time:

Setup Slack=Data Required Time−Data Arrival Time

Positive slack: Timing constraints are met.

Negative slack: Indicates timing violations, requiring optimization.

Clock Uncertainty:

Represents variations in clock signal generation and distribution. Key contributors are:

Clock Jitter: Deviation of the clock edge from its expected position, typically caused by noise or process variations.

Clock Skew: Difference in clock arrival times at different points in the design due to delays in the distribution network.

Margin: An additional buffer added to account for unforeseen variations.

Timing Violations and Their Causes:

Negative Slack: Occurs when the data arrival time exceeds the required time.

Common causes: Long critical paths, excessive capacitive loads, low drive-strength cells, or poorly optimized synthesis strategies.

Critical Paths: Paths with the least margin, most likely to violate timing constraints.

Timing Parameters:

Propagation Delay: The time taken for a signal to travel through a cell or net.

Transition Time (Slew): The time it takes for a signal to change from one logic level to another. Excessive slew can lead to increased delays and higher power consumption.

Latency: The overall delay in clock signal arrival from the source to the endpoint.

Clock Jitter:

Static Jitter: Caused by fixed delays in the clock path.

Dynamic Jitter: Results from variations in environmental conditions, such as temperature and voltage.

Optimizations to Address Timing Violations

If negative slack or timing violations are identified, the following steps can improve timing:

1. Cell Upsizing:

Replace cells with higher drive-strength variants to drive larger loads or reduce delays.

Example: Replacing a smaller inverter with a larger one.

2. Buffer Insertion:

Add buffers to drive high fan-out nets or rebalance skewed paths.

Helps distribute load evenly and reduces propagation delay.

3. Path Restructuring:

Reorganize logic to shorten critical paths.

May involve breaking long combinational paths into smaller segments.

4. Wire Sizing and Splitting:

Adjust wire widths or split long wires to reduce resistive and capacitive delays.

5. Adjusting Synthesis Strategies:

Modify synthesis settings, such as enabling cell resizing, buffering, or choosing a delay-focused strategy.

Once timing optimizations are applied:

The total negative slack (TNS) and worst negative slack (WNS) should improve or reach zero.


### Clock Tree Synthesis

Clock Tree Synthesis (CTS) is a critical step in the physical design process, responsible for generating an optimized clock distribution network. The primary goal of CTS is to deliver the clock signal from the source (clock pin or PLL) to all clock sinks (flip-flops, latches, or clock-gated cells) in a design with minimal skew and balanced latency. A well-designed clock tree ensures that all parts of the circuit operate synchronously, meeting timing requirements while minimizing power consumption and delay. Clock tree synthesis addresses variations in clock signal arrival times, optimizing the network for performance, power, and reliability.

The key parameters considered during CTS are clock skew, clock latency, and clock slew:

Clock Skew: The difference in clock arrival times at various sinks due to variations in routing or cell delays. Excessive skew can cause setup or hold timing violations.

Clock Latency: The total delay from the clock source to the sink. Minimizing latency ensures that the clock signal propagates quickly.

Clock Slew: The rate of change of the clock signal. Excessive slew can cause signal integrity issues and timing failures.

Types of Clock Distribution Structures:
1. H-Tree:

A symmetric clock tree where branches form an "H" shape at each level, ensuring equal delay paths to all sinks.

Advantages: Minimal skew and balanced paths.

Disadvantages: Higher power consumption due to uniform routing, even for non-critical sinks.
2. X-Tree (Binary Tree):

The clock signal is distributed hierarchically in a binary tree structure.

Advantages: Easier to implement, with reduced routing complexity.

Disadvantages: May introduce skew due to unequal path lengths.
3. Clock Mesh:

A grid of interconnected clock wires distributes the clock signal uniformly across the design.

Advantages: Very low skew, high robustness, and better tolerance to variations.

Disadvantages: High power consumption and routing resource usage.
5. Spine Tree:

Uses a central "spine" (a main horizontal or vertical clock trunk) from which branches distribute the clock signal to sinks.

Advantages: Simple and efficient for localized designs.

Disadvantages: May not scale well for large, complex designs.

6. Fishbone Clock Tree:

A variation of the spine tree with multiple spines and branches forming a "fishbone" structure.

Advantages: Better distribution for moderately sized designs.

Disadvantages: Requires careful balancing to avoid skew.

7. Hybrid Clock Tree:

Combines elements of different structures, such as H-tree with mesh or binary tree with spines, to balance skew, power, and resource usage.

Advantages: Customizable for specific design needs.

Disadvantages: Complexity in implementation.

Clock Gating Integration in CTS:

To further optimize power, CTS often integrates clock gating, where logic elements like AND or OR gates control clock propagation to specific regions of the design. This reduces switching power by turning off unused parts of the clock network. Clock gating, implemented either at the register or module level, is particularly beneficial in large designs with multiple functional blocks.

Challenges and Objectives of CTS:

Balancing Skew and Latency: The clock tree must ensure uniform arrival times across the design while minimizing overall delays.

Power Optimization: The clock network consumes significant dynamic power; hence, reducing switching activity and optimizing routing are key.

Scalability: CTS must handle designs with millions of clock sinks without compromising performance.

Signal Integrity: Avoiding issues like excessive slew or crosstalk is critical to ensuring reliable clock signal delivery.

Clock Tree Synthesis is a foundational process in digital design, ensuring that the clock network is robust, efficient, and meets stringent timing requirements.

Critical paths should meet timing constraints with sufficient slack margins.

Optimized slack ensures smoother progression to placement, CTS, and routing.


H-Tree :

![image](https://github.com/user-attachments/assets/cfee2351-3f9a-469e-a029-d224a7545e07)

Clock mesh :

![image](https://github.com/user-attachments/assets/6121ccd9-847a-4a42-a5cc-084345f4a091)


### Cross talk

Crosstalk in VLSI:

Crosstalk refers to the unwanted interference between signal lines in a VLSI design. This interference is primarily caused by capacitive or inductive coupling between adjacent metal traces or wires in the layout. As the integration density of VLSI components continues to increase, the risk of crosstalk also rises, leading to several potential issues.

![image](https://github.com/user-attachments/assets/aa78dda4-0870-4c97-bf36-0657038f7f73)

#### Impact of Crosstalk:

Data Corruption: Crosstalk can cause unwanted transitions in signal lines, potentially corrupting the data being transmitted. This is particularly problematic in high-speed designs where precise timing and signal integrity are crucial.

Timing Violations: Uncontrolled crosstalk can alter the timing characteristics of signals, resulting in delays or early transitions, leading to setup and hold time violations. This can cause flip-flops or latches to capture incorrect data, resulting in functional errors.

Increased Power Consumption: Crosstalk increases the switching activity of the interconnects, which directly impacts power consumption. When one signal line switches due to crosstalk, it can cause a neighboring line to switch as well, contributing to additional dynamic power consumption.

#### Mitigation of Crosstalk:

Layout and Routing Optimization: Careful layout design and routing of signal lines are essential to reduce the proximity of sensitive signal traces. Using larger spacing between critical signal lines or assigning dedicated layers to certain signal types can mitigate coupling effects.

Shielding: One effective method to mitigate crosstalk is through shielding, which involves placing grounded or highly resistive metal traces between adjacent signal lines to block or reduce the coupling.

Clock Distribution Strategies: Using a well-designed clock tree (CTS) can prevent crosstalk between the clock signal and data signals. Ensuring that the clock net does not interfere with critical signal lines is crucial for maintaining signal integrity.

Clock Gating: By employing clock gating techniques, which turn off the clock to portions of the circuit when not in use, dynamic power consumption can be reduced. This reduces the risk of crosstalk from constantly switching signals and helps maintain signal integrity.

#### Clock Net Shielding in VLSI:

The clock distribution network is essential for ensuring the synchronized operation of various parts of the chip. The clock signal must be propagated to all sequential elements (flip-flops, latches) in the design while minimizing clock skew and maintaining the integrity of the clock signal.

Purpose of Clock Shielding: Clock shielding in VLSI designs is implemented to reduce the interference between the clock signal and other signal lines, preventing the degradation of the clock’s integrity. The goal is to ensure that the clock signal reaches all clock sinks (e.g., flip-flops) with minimal skew while avoiding crosstalk with other signals in the design.

Shielding Techniques for Clock Nets:

Dedicated Clock Routing Layers: To prevent the clock signal from interacting with other signal lines, designers often allocate dedicated metal layers solely for clock routing. This reduces the chances of the clock signal coupling with nearby data or control signals.

Clock Tree Synthesis (CTS): The clock tree synthesis process includes algorithms that design an optimal clock distribution network, minimizing clock skew while ensuring that the clock signal is routed in a way that reduces interference. A well-balanced clock tree ensures that all clocked elements receive the clock signal at the correct time.

Buffer Insertion: Inserting buffers along the clock tree can help manage clock signal distribution more effectively, amplifying the clock signal where needed and ensuring uniform propagation across the chip. Buffers can also reduce signal degradation due to long interconnects or high capacitance.

Use of Guard Bands and Shielding Traces: Guard bands, or extra spacing between critical signal lines, and shielding traces placed between sensitive signals, help isolate the clock network from other interfering signals. These shields, often placed at strategic points in the layout, prevent crosstalk from degrading the clock signal.

Clock Domain Isolation: In modern VLSI designs, multiple clock domains are often used, each driven by a different clock source (e.g., different PLLs or clock sources for various functional blocks). It is essential to isolate these clock domains to prevent cross-domain interference that could lead to timing violations or data corruption. Shielding techniques, combined with proper clock gating and domain boundary management, help ensure that signals between different clock domains are isolated and do not interfere with each other. Additionally, ensuring proper synchronization mechanisms (e.g., clock domain crossing circuits) between domains is crucial to avoid metastability and data corruption when transferring signals between domains.

### Voltage Droop:

Definition: Voltage droop refers to a gradual decrease in voltage across the power delivery network (PDN) as current is drawn by the circuit. This drop occurs primarily due to the resistance and inductance in the power grid, leading to a reduction in the supply voltage seen by the functional blocks of the chip.

#### Causes:

High Current Demand: When large portions of the chip (especially during switching) require a significant amount of current, the voltage drops across the power grid due to the resistance of the interconnects and the power rails.

Power Delivery Network (PDN) Impedance: The impedance of the PDN, which includes the resistance and inductance of power traces, can cause a voltage droop when high-speed circuits draw current rapidly.

Capacitive Effects: Large capacitive loads (such as gate capacitance) switching together can draw significant current, causing voltage droop.

#### Impact:

Timing Violations: If the voltage drop is large enough, it can cause circuits to fail to meet the required voltage for proper logic levels, resulting in setup and hold time violations.

Reduced Performance: The reduced voltage seen by the circuits may not be sufficient for them to operate at their specified speed, leading to a reduction in overall performance.

Reliability Issues: If voltage droop is significant and persistent, it may cause the circuit to fail intermittently or permanently due to voltage levels falling below the operational thresholds.

#### Mitigation:

Decoupling Capacitors: Placing capacitors close to the power supply pins of cells can help buffer voltage changes and reduce the effects of voltage droop by providing local charge storage.

Improved PDN Design: Using wider power and ground traces, reducing the distance between the power supply and the circuits, and employing multiple layers for power distribution can reduce the resistance and inductance in the PDN, thereby limiting voltage droop.

Voltage Regulation: Implementing on-chip voltage regulators (such as LDOs or DC-DC converters) can help maintain a stable voltage supply, reducing droop.

### Voltage Bounce:

Definition: Voltage bounce refers to a transient variation in voltage caused by sudden switching events, particularly when there is a significant change in current demand. This is often observed in the power grid when switching devices or large blocks of logic cause a rapid fluctuation in the current, leading to momentary voltage fluctuations.

#### Causes:

Inductive Effects: When a signal switches, the current through the interconnects can change rapidly, and the inductive elements of the PDN can cause a brief fluctuation in voltage.

Simultaneous Switching Noise (SSN): Voltage bounce is often a result of simultaneous switching of multiple gates or blocks in the circuit, where each switch creates a current demand that adds up, causing a transient voltage spike.

Impedance Mismatch: Mismatched impedances in the power grid can amplify the effects of voltage bounce, causing significant fluctuations in the voltage seen by the circuits.

##### Impact:

Timing Violations: Just like voltage droop, voltage bounce can lead to timing errors. If the voltage temporarily rises above or drops below the threshold for logic level recognition, it can cause incorrect logic levels or metastability.

Signal Integrity Issues: Voltage bounce can interfere with the signals on nearby interconnects, causing noise and potential errors in signal transmission.

Increased Power Consumption: Voltage bounce may also lead to increased switching noise, which can cause higher dynamic power consumption.

#### Mitigation:

Decoupling Capacitors: Just as with voltage droop, decoupling capacitors are useful for minimizing voltage bounce by providing local charge storage to absorb current spikes.

Power Grid Design: Ensuring a low-impedance path for power delivery through careful layout and interconnect design helps to reduce voltage bounce. This includes improving power distribution and using techniques like power mesh or power straps to ensure low-resistance paths.

Controlled Switching: Slowing down the switching times of large blocks or using techniques like clock gating can reduce the rapid changes in current demand, minimizing voltage bounce.

On-Chip Noise Filters: Implementing noise filters or additional buffering in the PDN can help smooth out transient voltage changes caused by switching events.

![image](https://github.com/user-attachments/assets/dee8ea0a-c9a8-40c4-a049-28885aab7810)


### LAB CTS 

1. Handling Verilog File Modifications Post-Cell Replacement:

After attempting to reduce slack in previous runs, cell replacement techniques might have modified the netlist. This step ensures that the Verilog file reflects those changes. To update the Verilog file with the correct netlist:

Use the write_verilog command to write the modified Verilog file:

write_verilog <path_to_updated_file>

Afterward, re-run synthesis, floorplan, and placement to ensure the modifications are applied before moving on to the next steps.

2. Running Clock Tree Synthesis (CTS):

Once synthesis, floorplan, and placement are complete, and you've handled any necessary Verilog updates, you can proceed to Clock Tree Synthesis (CTS) to optimize the clock distribution network. This step uses the run_cts command in TritonCTS to balance clock skews and minimize delays across the clock network by introducing clock buffers or inverters. This helps in reducing clock skew and improving timing.

The command to run CTS is:

run_cts

After running CTS, you'll have updated slack values for the setup and hold timing checks. For example, after running CTS, your setup slack might be 10.71 ns and hold slack might be 0.24 ns, indicating that no violations occurred during timing analysis.

3. Post-CTS Analysis with OpenRoad:

After successful CTS, timing analysis with real clock propagation can be performed in OpenRoad. The following steps are carried out within the OpenLane flow:

Read LEF File: The LEF file contains the library's standard cells and their physical properties. This is crucial for timing analysis:

read_lef <path_to_merge.nom.lef>

Read DEF File: The DEF file describes the placement and routing information of the design:

read_def <path_to_def>

Write Database: Write the design database after reading the LEF and DEF files:

write_db pico_cts.db

Read the Design Database: Load the database into OpenRoad to perform timing analysis:

read_db pico_cts.db

Read Verilog File: Load the Verilog file that contains the netlist for the design:

read_verilog /home/vsduser/OpenLane/designs/picorv32a/runs/results/synthesis/picorv32a.v

Read Liberty File: The Liberty file contains timing and functional information for the synthesized cells, which are necessary for timing analysis:

read_liberty $::env(LIB_SYNTH_COMPLETE)

Read SDC File: The SDC file contains constraints, including clock definitions, for timing analysis:

read_sdc /home/vsduser/OpenLane/designs/picorv32a/src/my_base.sdc

Set Propagated Clock: Set the propagated clock for timing analysis. This step ensures that the timing analysis tool knows which clock is being used:

set_propagated_clock (all_clocks)

Perform Timing Checks: Finally, you can run timing checks to evaluate the design’s setup and hold timing. The following command generates a full report for the min and max path delay:

report_checks -path_delay min_max -format full_clock_expanded -digits 4

![image](https://github.com/user-attachments/assets/b8c0692b-1461-4f99-98e3-e71bdff3935d)

![image](https://github.com/user-attachments/assets/7ad9026c-5b0e-47ec-b22c-15614e9d402a)

SLACK is met!

{IMAGE_CREDITS : LECTURE FROM COURSE or AUTHOR'S SCREENSHOT}

