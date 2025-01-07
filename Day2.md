# Day 2 - Good floorplan vs bad floorplan and introduction to library cells

Floorplanning in the context of VLSI (Very-Large-Scale Integration) design refers to the process of determining the physical arrangement of components on a chip. It plays a critical role in ensuring the functionality, performance, and manufacturability of the integrated circuit (IC). A good floorplan optimizes key parameters like area, power, performance, and thermal management.

Key Components of Floorplanning:

-> Cells and Blocks: These are the fundamental building units of the design. Cells can be logic gates, flip-flops, or other standard cells, while blocks are groups of cells or larger functional units.

-> Macros: These are pre-designed functional units, such as memory or complex modules, which are placed on the chip during floorplanning.

-> Power and Ground Distribution: Ensuring proper routing of power and ground to all components is a key part of floorplanning to prevent power issues (usually seperately done as powerplan). 

Floorplan Parameters:
Aspect Ratio: The aspect ratio is the ratio of the chip's length to its width. A good aspect ratio ensures a balanced distribution of the area and helps in minimizing routing congestion. The aspect ratio should generally be kept close to 1:1 for optimal performance, but it depends on the specific design goals.

Aspect Ratio = Length / Width: If the chip’s dimensions are square-like (i.e., equal length and width), the aspect ratio is 1:1. A long, narrow chip would have a high aspect ratio (e.g., 3:1), whereas a wider chip might have a lower aspect ratio (e.g., 1:2).

Floorplan Types:

-> Row-based floorplan: Components are organized in horizontal rows. This is common for standard cell layouts.

-> Cluster-based floorplan: Groups related components (or blocks) into clusters, optimizing them together.

->Core-based floorplan: Components are arranged within a predefined core region, and peripheral components are placed around the core.

![image](https://github.com/user-attachments/assets/6fdfebe2-3aca-47ac-bed3-b2d05b6c7f97)


Objectives of Floorplanning:

-> Minimize wire length: Shorter wire connections reduce signal delay and power consumption.

-> Achieve balanced routing: The design should avoid congestion and excessive wire crossings, which can impact performance.

-> Thermal management: Proper placement can help distribute heat evenly across the chip.

-> Area optimization: Maximize the use of available chip area while minimizing unused space.

Utilization Factor = Area occupied by netlist/Total area of the core


### Pre-placed Cells:
Pre-placed cells refer to certain large, predefined blocks or modules in a circuit design that are placed early in the physical design flow, typically before the general placement and routing stages. These cells include complex functional units like memory blocks, PLLs (Phase-Locked Loops), or other highly specialized components that have been designed and optimized separately. They are pre-configured to meet specific performance, power, or area requirements and are intended to be reused across different designs, saving both time and effort.

These cells are crucial for several reasons:

Performance Optimization: Pre-placing certain cells can help in optimizing the overall performance of the design. For instance, memory blocks are often large and complex, and placing them early can ensure they are located in the most efficient part of the chip. This can reduce the delay and the overall wire length for connecting other components, thus improving speed.

IP Integration: Pre-placed cells often consist of Intellectual Property (IP) blocks that are already verified and validated. These blocks can be integrated into various designs without the need to redesign the modules from scratch. This accelerates the design process and ensures consistency and reliability across different designs.

Power Optimization: By pre-placing power-hungry modules such as memory, complex functional units, or processors, designers can optimize power delivery. This reduces the likelihood of power supply issues, ensuring that these critical blocks have the necessary resources to function without fluctuations in voltage or current.

![image](https://github.com/user-attachments/assets/3162856d-0a96-4faf-bd51-307599fa36db)

### Decoupling Capacitors:

Decoupling capacitors play an essential role in maintaining a stable power supply, particularly in circuits with high-speed switching elements or complex pre-placed cells. A decoupling capacitor is a type of capacitor placed strategically between the power supply and the integrated circuit (IC). Their primary purpose is to smooth out fluctuations in the power supply voltage caused by sudden current demands during switching events.

Key Functions of Decoupling Capacitors:

Voltage Stabilization: When high-speed circuits or modules like processors, memory, or pre-placed functional blocks switch between states, they can cause rapid changes in current demand. These changes can lead to voltage drops or noise that affect the reliability and stability of the design. Decoupling capacitors help mitigate these effects by providing a localized source of charge. They store electrical energy during periods of low activity and release it when there’s a surge in current demand, thereby stabilizing the power supply voltage.

Charge Reservoir: Decoupling capacitors act as charge reservoirs. When the circuit is idle or not switching, the capacitor charges up from the power supply. However, when the circuit begins to switch or experience high current demand, the capacitor discharges, providing the required current to the circuit. This ensures that the switching transients are met without causing voltage sag or power interruptions.

Noise Filtering: Besides stabilizing voltage, decoupling capacitors filter out high-frequency noise from the power supply. These fluctuations or noise spikes, which can be caused by high-frequency switching activities, can interfere with the operation of the circuit. By placing capacitors near sensitive pre-placed cells or blocks, this noise is suppressed, maintaining the integrity of the signal.

Proximity to Critical Components: The effectiveness of decoupling capacitors depends on their placement. Ideally, they should be placed as close as possible to the power supply pins of the pre-placed cells. This minimizes the impedance and resistance of the traces between the capacitor and the cell, ensuring quick and efficient charge delivery during switching events. Additionally, multiple capacitors may be used to target different frequencies, with smaller capacitors handling higher-frequency noise and larger ones addressing lower-frequency fluctuations.

![image](https://github.com/user-attachments/assets/0292132d-c635-46b4-b63b-b0e71366ebda)

Importance in Power and Performance:

Power Management: Decoupling capacitors contribute to better power management by reducing the demand on the central power supply, thereby enhancing efficiency.

Preventing Power Supply Instability: Without these capacitors, circuits would be more vulnerable to power supply instability during heavy switching activity, which could lead to system malfunctions or crashes.

Preserving Signal Integrity: By filtering out noise, these capacitors help maintain the integrity of signals and ensure the smooth operation of high-speed circuits.

### Pin Placement 

Pin placement in physical design is a crucial aspect that directly influences the performance, manufacturability, and reliability of a chip or circuit board. It involves strategically positioning the input/output (I/O) pins to optimize signal routing, minimize delays, and reduce interference. Proper placement ensures signal integrity by shielding sensitive signals from noisy power pins, preventing signal degradation. Moreover, it allows for the even distribution of power across the chip, avoiding voltage drops and hotspots that could affect performance. Heat management is another important consideration, as high-power components must be positioned for effective heat dissipation. Placing buffers near the I/O ports is a key technique used to improve signal driving capabilities and minimize the impact of long routing delays. Buffers act as intermediaries between the I/O pins and internal circuits, ensuring that signals are properly amplified and driving strength is maintained, especially in high-speed designs or when driving multiple load lines. Pin placement also must align with packaging constraints, ensuring compatibility with external connectors and packaging standards. Additionally, grouping related signals together helps simplify routing and improves design efficiency. By considering all these factors, including proper buffer placement, pin placement enhances the overall design’s reliability, manufacturability, and ease of testing.

![image](https://github.com/user-attachments/assets/d2f473d5-4a8f-4d31-bc17-021631dc0e13)

{IMAGE CREDITS: Corpus ID: 14807484 "Timing-constrained I/O buffer placement for flip-chip designs"}

### Power Planning

Power planning is a vital aspect of integrated circuit (IC) design, ensuring efficient power delivery, minimizing power consumption, and maintaining the chip’s performance and reliability. It involves designing a comprehensive power distribution network (PDN) that effectively distributes power across the chip while minimizing voltage drops and noise. A key component of power distribution is the power mesh, which is a network of metal layers that form the power grid, connecting the power (VDD) and ground (VSS) pins to all parts of the chip. The power mesh ensures that power is delivered evenly to all modules, balancing the power distribution and reducing the risk of localized voltage fluctuations. In addition to the power mesh, the grid system is composed of horizontal and vertical metal layers, which create a structure for the PDN. The grid is designed to handle the current load and maintain a stable voltage level across the chip.

![image](https://github.com/user-attachments/assets/533e6b9d-ae5b-4037-87c2-48e35709707e)

{IMAGE CREDITS: VLSI Backadventure}

The use of power rails and rings is another important consideration in power planning. Power rails are metal paths that connect the power and ground grids to the components, ensuring that power is delivered efficiently to various regions of the chip. Rings, which are continuous paths of metal surrounding the core of the chip, are often used to connect the outermost power and ground pins, ensuring reliable power distribution even at the chip’s boundaries. These rings act as a buffer against potential voltage drops, providing a stable power supply to the entire chip. 

### Steps to run floorplan 

Run the floorplan using the command "run_floorplan". 

After successful completion, in order to see the floorplan, one can  invoke the magic tool; The inputs required are tech file, lef file and def file.

Command: "magic -T /home/../../sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32a.def & "

![image](https://github.com/user-attachments/assets/a6e55a8c-8624-4000-801d-85b306d06343)

The standard cells are still located at the origin and not yet placed.

![image](https://github.com/user-attachments/assets/3b9758f5-1ae7-4a13-8245-31c847e4d5bf)

### Library Binding and Placement 

he first step in physical design after logical synthesis/floorplan is binding the netlist to physical cells, where the logical gates in the netlist are mapped to actual physical cells with real dimensions. The netlist consists of various gates that, while represented in the schematic as abstract logical elements, are typically rectangular or square in shape when implemented in production. This process is essential because the cells in the schematic do not directly translate to the final physical design—each gate is associated with a specific cell from a pre-defined library of cells.

These cells are sourced from a library, often referred to as a "cell library" or "standard cell library." The library contains a wide variety of pre-designed cells with different functionalities, shapes, and sizes, and is a crucial resource in physical design. In addition to basic logic gates, the library may contain complex functional units such as multiplexers, flip-flops, and memory blocks. Each cell in the library comes with real-world dimensions and performance characteristics, such as delay, power consumption, and area, that are necessary for accurate physical design.

The library also includes cells of varying sizes that perform the same function. Larger cells tend to have lower resistance due to their increased width, which leads to reduced delay and lower power consumption. However, they also occupy more area on the chip. Conversely, smaller cells take up less space but typically have higher resistance, which can result in increased delay and higher power consumption. Therefore, the selection of cell sizes is an important design decision, as it involves a trade-off between speed (performance), power, and area (PPA).

Cell placement is a crucial step in the physical design of integrated circuits (ICs), where the components or cells are arranged on the chip to ensure efficient routing, optimal performance, and minimal area usage. Proper cell placement directly impacts the chip’s speed, power consumption, and overall manufacturability. The goal is to place cells in a way that minimizes signal delays, power dissipation, and routing congestion while meeting the design specifications.

In the context of cell placement, there are several specialized techniques designed to optimize the design based on different constraints and objectives. These techniques are aimed at ensuring that the final placement of cells satisfies the design requirements for timing, power, area, and congestion. Some of the key types of placement strategies include Congestion-Aware Placement, Timing-Aware Placement, and other similar approaches.

1. Congestion-Aware Placement:
 
Congestion-aware placement focuses on reducing congestion in the routing network, which occurs when too many routing wires are required in a small area. High congestion can lead to longer routing delays, increased power consumption, and potential violations of design rules. In congestion-aware placement, the algorithm attempts to distribute cells across the chip in such a way that areas of high routing congestion are avoided. By spreading cells out more evenly or placing them in areas with more available routing resources, this technique helps minimize the chances of congestion and facilitates a smoother routing process in the later stages of design.

2. Timing-Aware Placement:

Timing-aware placement optimizes the placement of cells based on the critical timing paths of the design. The goal is to minimize the delay between critical paths, which are the longest paths that determine the overall speed of the circuit. In timing-aware placement, cells that are part of critical paths are placed closer together to reduce wire lengths and interconnect delays, thereby improving the overall timing of the design. This approach helps ensure that the chip meets its timing requirements and operates at the desired clock speed.

3. Power-Aware Placement:

Power-aware placement focuses on minimizing power consumption during the placement stage. It ensures that power-hungry cells, such as those with high switching activity or large fan-out, are placed in locations where power distribution is optimal and efficient. By placing these cells closer to power and ground rails or balancing them across the chip, this technique helps reduce power grid voltage drops and minimizes excessive power consumption. It can also involve grouping cells based on their power characteristics to avoid hotspots.

4. Area-Aware Placement:

Area-aware placement aims to minimize the total area occupied by the design while meeting other constraints. It ensures that the cells are placed in such a way that the chip’s area is used efficiently without wasting space. This method typically balances the trade-off between reducing area and meeting timing, power, and congestion constraints. It involves compacting the design and reducing unnecessary gaps between cells.

To do placement, type the command "run_placement". 

To view the placement of the cells using magic tool, use the following command in the  directory runs/date/results/placement

Command: "magic -T /home/../../sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def read picorv32a.def & "

![image](https://github.com/user-attachments/assets/a5a924f0-f405-442d-a677-89f707c42077)

### CELL DESIGN AND CHARACETRIZATION FLOWS

Cell Design and Characterization Flows play a critical role in ensuring that the physical cells used in integrated circuits (ICs) meet the required specifications and perform optimally. A cell library is essentially a collection of pre-designed, reusable cells, each designed for specific functions, sizes, and threshold voltages. These cells serve as building blocks for larger designs, and the process of cell design and characterization ensures that each cell performs efficiently under real-world conditions.

Inputs:

PDKs (Process Design Kits): These include crucial information and resources, such as DRC (Design Rule Check) and LVS (Layout Versus Schematic), ensuring that the design adheres to fabrication constraints. They also contain SPICE models, which define the electrical characteristics of the devices, and other data essential for designing and verifying the cells.

Library & User-Defined Specifications: The library provides the fundamental cells with varying sizes, functionalities, and threshold voltages. These cells are tailored to meet certain design requirements such as speed, power, and area. User-defined specifications may include additional constraints on these parameters.

Design Steps:

Circuit Design: This step involves the creation of the cell's schematic, representing the logic function that the cell will perform. The schematic should meet the design goals such as timing, power, and functionality.

Layout Design: The layout design is a key part of the process, where the physical representation of the cell is generated. Techniques like Euler's path and stick diagrams are used for efficiently designing the layout, ensuring minimal area, and optimal routing for the electrical signals.

Extraction of Parasitics: Once the layout is designed, parasitic elements (such as capacitance and inductance) are extracted, as these can affect the performance of the cell. The extraction process ensures that the physical design aligns with the electrical behavior of the cell.

Characterization: Characterization involves evaluating the cell’s timing, noise, and power characteristics. This process ensures that the cell meets its performance requirements under various operating conditions.

Outputs:

CDL (Circuit Description Language): This is a netlist representation of the circuit that describes how the cells are connected and how they function.

LEF (Library Exchange Format): LEF files describe the physical aspects of the cells, including their pin locations and dimensions.

GDSII: This file format is used to define the geometric data of the cell's layout and is used for mask generation during fabrication.

Extracted SPICE Netlist (.cir): This netlist includes parasitic elements that were extracted from the layout, providing a more accurate representation for post-layout simulations.

Timing, Noise, and Power .lib Files: These library files contain the characterized data of the cell, which includes timing, noise margins, and power consumption. These files are crucial for tools that perform synthesis, static timing analysis, and other verification tasks.

Standard Cell Characterization Flow:

The Standard Cell Characterization Flow involves the following steps to generate the necessary performance, noise, and power models for each cell:

Read in the Models and Tech Files: Import the necessary SPICE models and technology files, which define the electrical behavior and process rules for the cells.

Read Extracted SPICE Netlist: Load the SPICE netlist that includes parasitic information from the layout, ensuring accurate simulations for characterization.

Recognize Behavior of the Cells: Understand the logical and electrical behavior of the cells to ensure that they meet the design objectives. This step involves analyzing the timing, functional behavior, and interactions of the subcircuits.

Read the Subcircuits: Extract and understand the subcircuits used in the cell, which make up its functional components.

Attach Power Sources: Attach appropriate power and ground sources for the cells during the simulation to model their behavior accurately.

Apply Stimulus to Characterization Setup: Introduce input signals to the cells to test their response and analyze timing, power, and noise characteristics under various conditions.

Provide Necessary Output Capacitance Loads: Specify the appropriate load capacitance for the simulation, as this can impact the timing and power of the cell.

Provide Necessary Simulation Commands: Set up simulation commands to evaluate the timing, power, and noise characteristics.

These steps are combined into a configuration file, which is then input into characterization software like GUNA. This software automates the process of generating the necessary timing, noise, and power models for the cells. The resulting .lib files are categorized into:

Timing Characterization: Containing data on delays, setup/hold times, and other timing-related metrics.

Power Characterization: Containing information on the dynamic and static power consumption of the cells.

Noise Characterization: Containing data on noise margins and susceptibility to noise.

Through this comprehensive flow, cells are characterized to ensure they function correctly and efficiently within the overall design. This step is critical for ensuring the robustness and reliability of the final integrated circuit.

### Timing Characterization in Standard Cell Design

In standard cell characterization, timing plays a crucial role in ensuring that the cells meet the desired speed and performance requirements. The timing characterization process involves measuring various delay metrics and transition times to define how signals propagate through the cells and how quickly they respond to changes in input signals. Here, we focus on the concepts of timing thresholds, propagation delay, and transition time, which are fundamental to timing analysis.

Timing Threshold Definitions:

In timing characterization, several key timing thresholds are defined to determine how the signals are measured during the characterization process:

slew_low_rise_thr: This is the 20% value of the signal's transition during a rise.

slew_high_rise_thr: This is the 80% value of the signal's transition during a rise.

slew_low_fall_thr: This is the 20% value of the signal's transition during a fall.

slew_high_fall_thr: This is the 80% value of the signal's transition during a fall.

in_rise_thr: The 50% value of the signal transition for the input during a rise.

in_fall_thr: The 50% value of the signal transition for the input during a fall.

out_rise_thr: The 50% value of the signal transition for the output during a rise.

out_fall_thr: The 50% value of the signal transition for the output during a fall.

These thresholds are essential for defining the time points at which signal transitions are considered to have occurred. They help in determining the signal’s response time and how quickly it changes from one state to another.

Propagation Delay and Transition Time:

Propagation Delay:

Propagation delay is a critical metric in timing analysis. It refers to the time difference between the point when an input signal reaches 50% of its final value and the point when the output signal reaches 50% of its final value. In essence, it measures the time it takes for a signal to propagate through a gate or a cell and affect its output.

The propagation delay is calculated as:

Propagation Delay = time(out_thr)−time(in_thr)

Where:

time(out_thr): The time at which the output reaches the threshold value.

time(in_thr): The time at which the input reaches the threshold value.

It’s essential to choose proper threshold values to avoid negative delays, which may occur if the thresholds are set incorrectly or if the input signal has poor slew characteristics. Even with good threshold values, if the slew rate is poor, the delay might still be positive or negative.

Transition Time:

Transition time refers to how long it takes for a signal to change between different states, and it is typically measured between certain percentage values of the signal’s levels. The time is measured from a lower percentage threshold to a higher percentage threshold.

Rise Transition Time: This is the time it takes for the signal to transition from the low rise threshold (20%) to the high rise threshold (80%). It is given by:

Rise Transition Time = time(slew_high_rise_thr)− time(slew_low_rise_thr)

Fall Transition Time: Similarly, the fall transition time is the time taken for the signal to transition from the low fall threshold (20%) to the high fall threshold (80%). It is calculated as:

Fall Transition Time=time(slew_high_fall_thr)−time(slew_low_fall_thr)

These transition times are crucial for understanding how quickly the signal changes states, which directly impacts the speed of the circuit. Transition time also influences the cell’s overall performance and the propagation delay.

Summary:

In timing characterization:

Propagation Delay measures the delay in signal propagation through a cell and is calculated as the difference between the input and output threshold times.

Transition Time measures the time it takes for the signal to transition between states, defined by the rise and fall thresholds.
