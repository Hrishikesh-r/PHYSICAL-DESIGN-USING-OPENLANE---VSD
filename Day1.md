# PHYSICAL-DESIGN-USING-OPENLANE---VSD

## Day 1 - Introduction, PD flow, RISC-V Architecture & Getting familiar with Openlane

### Introduction :
VLSI (Very Large Scale Integration) refers to the process of creating integrated circuits (ICs) by combining millions or billions of transistors onto a single chip. VLSI enables the development of compact, efficient, and high-performance electronic systems. The chip design process typically involves several key stages:

Specification: Defining the functional, performance, and physical requirements of the chip, such as speed, power consumption, area, and interfaces.

Architectural Design: Designing the overall structure of the chip, including the selection of processing units, memory hierarchies, and interconnections.

RTL Design: Using Hardware Description Languages (HDLs) like Verilog or VHDL to describe the chip's functionality at the Register Transfer Level (RTL).

Synthesis: Converting the RTL design into a gate-level netlist using logic synthesis tools, optimizing for power, performance, and area (PPA).

Physical Design: Laying out the physical implementation of the chip, including placement, routing, clock tree synthesis, and timing closure.

Verification: Ensuring the design meets specifications through simulation, formal verification, and hardware emulation.

Fabrication: Sending the finalized design to a semiconductor foundry for manufacturing, involving photolithography and other processes to create the chip on a silicon wafer.

Testing: Validating the fabricated chip's functionality and performance using Automated Test Equipment (ATE).

Chip design is a highly iterative process that balances trade-offs between performance, power efficiency, cost, and time-to-market. It is fundamental to creating modern electronic devices such as smartphones, computers, and IoT gadgets.


A Typical Design Flow is shown below:
![image](https://github.com/user-attachments/assets/5b7714d0-aede-4212-b84e-2533ea538b7e)

{IMAGE CREDITS:Prof. W. Rhett Davis NC State University } 

### Front-End (Logic Design)
1. RTL Coding (Register Transfer Level)

-> To describe the functionality of the chip using Hardware Description Languages (HDLs) like Verilog or VHDL.

-> Specifies how data flows between registers and how operations are performed at each clock cycle.

-> Focuses on the design's behavior, modularity, and hierarchy.

-> Captures logic such as combinational circuits, sequential circuits, and finite state machines (FSMs).

-> Verification: The RTL code is validated through simulation to ensure functional correctness.

2. Synthesis
   
-> To convert the RTL code into a gate-level netlist, which maps the design to a specific technology library (e.g., standard cells).

Steps in Synthesis:

-> Translation: Transforms HDL code into a generic, intermediate representation.

-> Optimization: Minimizes logic gates and reduces timing, power, and area while maintaining functionality.

-> Technology Mapping: Maps the optimized design to the target library cells.

### Back-End (Physical Design Flow)

Physical design in VLSI is the process of transforming a synthesized netlist into a geometrical representation of the chip, which can be manufactured. This stage ensures the design meets timing, power, area, and manufacturability constraints.

1. Partitioning and Floorplanning:
Divide the chip into smaller, manageable blocks (if needed).
Define the placement of blocks, I/O pins, and power grid.
Allocate space for standard cells, macros, and interconnects.

2. Power Planning:
Design a robust power distribution network (PDN) to deliver power to all components.
Ensure the design meets IR drop and Electromigration (EM) constraints.

3. Placement:
Place standard cells within the allocated areas while optimizing for timing, power, and area.
Perform legalization to resolve overlaps.

4. Clock Tree Synthesis (CTS):
Design and optimize the clock distribution network to minimize clock skew and latency.
Balance power consumption and ensure the clock signal reaches all sequential elements simultaneously.

5. Routing:
Connect all the cells and macros using metal layers according to the netlist.
Optimize for signal integrity, congestion, and manufacturability.
Perform global and detailed routing.

6. Design Rule Check (DRC) and Layout Versus Schematic (LVS):
Verify that the layout complies with the foundry’s design rules (DRC).
Ensure the layout matches the schematic by comparing the netlist (LVS).

7. Timing Analysis and Optimization:
Perform Static Timing Analysis (STA) to ensure all paths meet setup and hold timing requirements.
Fix timing violations by resizing cells, adding buffers, or adjusting routing paths.

8. Signal Integrity (SI) Analysis:
Analyze and resolve issues like crosstalk and voltage noise that could degrade signal quality.

9. Power Analysis and Optimization:
Check power consumption, IR drop, and thermal issues.
Optimize the design to meet power budgets.

10. Final Verification:
Conduct comprehensive checks for manufacturability, performance, and reliability, including DRC, LVS, and Parasitic Extraction (PEX).

### RISC V Architecture

RISC-V is a standard Instruction Set Architecture (ISA) that provides a standardized interface between software and hardware. It defines the basic commands and data-handling mechanisms that a processor can execute, serving as the "language" of a computer. When a program written in a high-level language such as C is executed on a hardware platform, it undergoes several translation steps:

-> High-Level Language to Assembly Code: The program is compiled into an intermediate, human-readable format called assembly language, specific to the target ISA like RISC-V.

-> Assembly Code to Machine Code: The assembler converts assembly instructions into binary machine code (1s and 0s), which the hardware can interpret.

-> Execution on Hardware: The machine code runs on a processor core (e.g., PicoRV32) implemented in a hardware description language like Verilog or VHDL.

System Software Components

1. Operating Systems (OS):

-> Manage hardware resources like memory, processors, and input/output devices.
-> Provide low-level system functionality required for running user applications.
-> Accept instructions in high-level programming languages and process them further for execution.

2. Compilers:

-> Convert high-level language code into assembly instructions tailored for the specific ISA of the target processor (e.g., RISC-V).

-> Generate executable files that act as an abstraction layer between software and hardware.

3. Assemblers:

-> Translate assembly code into machine-level instructions.

-> Produce binary files directly executable by the processor.

These machine level instructions are fetched by the processor, decoded, perform the required execution, update & write back to memory. These are executed through various blocks which are designed using HDL. Below is a high level representation of the architecture of a microprocessor.

![image](https://github.com/user-attachments/assets/f570f735-d14b-43d2-8841-db739f293dbe)

{IMAGE CREDITS:Prof. Eric Rotenberg NC State University } 

### Getting familiar with Openlane

OpenLANE is an open-source, fully-automated RTL-to-GDSII (Register Transfer Level to layout) flow designed for digital ASIC design. It integrates various open-source EDA tools to provide a comprehensive solution for chip design, from synthesis to layout generation.

Key Features of OpenLANE

End-to-End Automation: Automates the entire physical design process from RTL synthesis to layout (GDSII) generation.

Modularity: Supports individual tools for specific design stages, allowing customization and exploration.

Open-Source Tools Integration: Includes tools such as:

Yosys: RTL synthesis.

ABC: Logic optimization.

OpenROAD: Placement and routing.

Magic: Layout visualization and design rule checking (DRC).

KLayout: GDSII manipulation and viewing.

Technology Independence: Works with different open-source process design kits (PDKs), such as SkyWater 130nm (SKY130).

Design Exploration: Allows users to tune constraints and configurations for PPA (Performance, Power, and Area) optimization.


#### OpenLane Flow

![image](https://github.com/user-attachments/assets/72b057aa-1217-4d74-820b-3f869b619294)

The whole documentation can be found here [https://github.com/The-OpenROAD-Project/OpenLane]. OpenLane abstracts the underlying open source utilities, and allows users to configure all their behavior with just a single configuration file.

Steps to Invoke tool :

1. Go to the openlane directory using the below command :

![image](https://github.com/user-attachments/assets/f1af31d7-2e07-4d39-a94d-9155b8df150e)

2. Invoke the Docker using :

![image](https://github.com/user-attachments/assets/31656683-02d6-4d69-b25b-d6f5c3271a8d)


3. Invoke the Open with the help of :

![image](https://github.com/user-attachments/assets/1ba4e3a8-d055-4b1d-8fe8-a648cc744af1)


4.  type the command "package require openlane 0.9"

5.  The design considered in this course is picorv32a, hence load this design to tool using the command "prep -design picorv32a".

![image](https://github.com/user-attachments/assets/d2487e65-b10b-445d-a53d-39c1e7189ac5)

6. Complete synthesis using the command "run_synthesis"

7. Analyze the synthesis results

![image](https://github.com/user-attachments/assets/93b17e74-a1c1-4915-a652-0397ac61a4d0)


Flop ratio = Number of D Flip flops/ Total Number of cells  = 1613 /14876  =  9.7415%       


