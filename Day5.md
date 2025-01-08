# Day 5 -  Final steps in RTL2GDS

## Routing 
Routing is a crucial step in the design process of VLSI circuits, responsible for creating the physical interconnections between various components (e.g., logic gates, flip-flops, and memory cells). The objective of routing is to establish a valid and efficient path for signals to travel between these components while adhering to design rules and avoiding obstacles. Routing algorithms take the source and target pins, typically located on the chip, and aim to find the most optimal path for the connection.

### Maze Routing and the Lee Algorithm

Maze routing is one of the most fundamental methods for routing in VLSI design. The Lee algorithm is a widely used maze routing algorithm that efficiently solves the problem of finding the shortest path between two points in a grid-like structure, which is typical in VLSI layout design. The algorithm works well for both global and detailed routing, ensuring that the routing process adheres to constraints like spacing rules and layer restrictions.

#### Working of Lee's Algorithm:

1. Grid Setup:

A grid, similar to the layout created during cell customization, is established where each point corresponds to a potential location for routing.

Obstacles, such as existing metal layers or other components, are identified on the grid as "blocked" regions where routing cannot pass.

2. Start and Target Points:

The source (starting point) and target (destination point) are marked on the grid. The goal is to find the shortest possible path between these two points.

3. Labeling:

Lee's algorithm works by labeling the neighboring cells around the source point. Initially, the source point is labeled as 0 (starting point), and its immediate neighbors are assigned the value 1. The algorithm then propagates through the grid, incrementing the values of neighboring cells (e.g., 2, 3, 4, etc.) as it moves further away from the source.

The algorithm continues this process until it reaches the target, incrementing the distance labels along the way.

4. Pathfinding:

Once the algorithm reaches the target, it traces the path from the target back to the source by following the cells with the lowest labels.

This path is the shortest possible route between the source and target.

5. Path Shape:

The algorithm can produce different types of paths, such as L-shaped or zigzag-shaped routes. Lee's algorithm tends to prioritize L-shaped paths when they are available, as they typically represent more efficient routing with fewer turns.

If L-shaped paths are unavailable due to obstacles or other constraints, the algorithm may resort to zigzag paths.

6. Termination:

The algorithm terminates once the target is reached, and the optimal routing path is identified. If no path exists (due to obstacles blocking all possible routes), the algorithm will indicate that no solution is found.

![image](https://github.com/user-attachments/assets/0637ac59-b424-452d-9638-94e3f3084a78)

#### Limitations of Lee's Algorithm
While the Lee algorithm is effective for solving routing problems, it has some limitations:

1. Time Complexity:

The time complexity of Lee's algorithm is proportional to the size of the grid (O(N²) in the worst case), which can become computationally expensive when dealing with very large grids or designs with millions of pins. This is especially true in modern VLSI designs with high integration densities.

2. Grid Expansion:

The algorithm expands through the grid layer by layer, which can result in inefficient memory usage, especially when dealing with large, complex routing tasks. In certain scenarios, this can make Lee’s algorithm less efficient compared to other, more advanced routing algorithms.

3.Limited Path Optimization:

Lee's algorithm guarantees finding the shortest path but does not optimize for factors like wire capacitance, resistance, or timing delay, which are critical in VLSI routing. It is purely a geometric pathfinding algorithm, which limits its applicability in more sophisticated routing scenarios.

The Lee algorithm remains a foundational technique for maze routing in VLSI design, effectively finding the shortest path between two points in a grid. While it is simple and guarantees finding the optimal path in terms of distance, its computational cost and limited optimization for factors like delay and capacitance make it less suitable for large, modern designs. By understanding its strengths and weaknesses, VLSI designers can choose the appropriate routing algorithm or combine it with other techniques to achieve efficient routing in complex designs.

## Design Rule Check 

Design Rule Check (DRC) is a critical verification step in the physical design flow of VLSI circuits. It ensures that the layout of a design adheres to the manufacturing process constraints provided by the semiconductor foundry. DRC verifies that all geometrical and electrical properties of the layout satisfy the design rules, which are essential for ensuring that the chip can be manufactured correctly and will function as expected.

DRC helps to identify potential issues that may lead to chip failure, such as:

1. Physical violations (e.g., too narrow or too wide wires)
   
2. Electrical failures (e.g., insufficient spacing causing shorts)

3. Manufacturing problems (e.g., issues related to layer alignment or via design)

### Key Types of DRC Checks

Design Rules for Physical Wires:

1. Minimum Width of the Wire: Ensures that the width of a wire (or metal line) does not fall below a specified minimum value. If the wire is too thin, it may not be able to carry the required current, leading to reliability issues.

2. Minimum Spacing Between Wires: This rule ensures that there is enough space between two adjacent wires to prevent short-circuiting. If wires are placed too close together, they may overlap, causing an electrical short.
  
3. Minimum Pitch of the Wire: The pitch is the center-to-center distance between two consecutive wires. A smaller pitch allows more wires to be placed in the same area, increasing integration, but it also increases the chance of signal interference and crosstalk.

4. Via Rules:

4(a). Via Width: Defines the minimum size of the via (a metal connection between layers), ensuring that it is large enough to carry the current without resistance or reliability issues.

4(b). Via Spacing: Ensures that vias are spaced apart sufficiently to avoid the risk of electrical shorts and maintain the integrity of the routing.

5. Metal Layer Considerations:

Metal Layer to Upper Metal Layer: To avoid signal shorts due to adjacent metal layers, it may be necessary to route a signal from one metal layer to another using a via. The design must ensure that the via rules are followed to prevent short-circuiting between metal layers.

6. Signal Integrity:

6(a). Minimum Width of the Metal Layers: To avoid signal degradation and ensure proper current carrying capability, the metal lines must have a sufficient width.

6(b). Spacing Between Power and Ground Rails: Ensures that the distance between the power (VDD) and ground (VSS) rails is sufficient to prevent power grid-related failures, such as voltage droop or crosstalk.

### Key DRC Violations:
1. Short Circuits: Occur when wires or metal layers are too close, resulting in unintended connections between signals, which can lead to malfunction.

2. Open Circuits: Happen when there is insufficient connection between components, causing certain parts of the circuit to remain disconnected.

3. Metal Overlaps: These occur when metal layers intersect inappropriately or extend too far, leading to potential shorts or manufacturing errors.

4. Under/Over-Sized Vias: If vias are too small, they cannot carry sufficient current, while oversized vias can cause unnecessary capacitance and delays.

![image](https://github.com/user-attachments/assets/07311292-cae5-4cf1-b6a9-6a9d374e2301)

### DRC Flow in Physical Design:

Design Setup:

The DRC setup begins by defining the design rules based on the foundry's process technology. These rules specify the minimum allowable values for metal widths, via sizes, and spacing.

The design environment should be configured to incorporate the correct process technology files, including the tech file (technology file) and the DRC rule deck.

DRC Checking:

The layout is passed through a DRC tool (such as Calibre, Mentor Graphics DRC, or Cadence DRACULA) to verify that the design adheres to the defined process rules.

The tool checks all aspects of the layout, from wire widths and spacing to layer-to-layer interactions and via configurations.

Violations are flagged by the tool and reported to the designer for review.

Violation Resolution:

If violations are found, the designer must address them by adjusting the layout, which may include widening wires, increasing spacing, or modifying the via structure.

These changes must be re-checked to ensure that no new violations are introduced.

Iterative Process:

DRC checking is an iterative process, often repeated multiple times during the design flow as the layout is modified or optimized. The designer continues to resolve violations until all DRC rules are satisfied.

## Power Delivery Network

In the OpenLANE flow, Power Distribution Network (PDN) generation is a critical step that is carried out after Clock Tree Synthesis (CTS) and post-CTS Static Timing Analysis (STA). Unlike the general ASIC flow, where PDN is often generated as part of the floorplanning stage, OpenLANE requires that the PDN be generated later in the design process, specifically after these steps have been completed.

PDN Generation Process:

PDN Generation Command: The PDN is generated using the command gen_pdn in OpenLANE. This command creates the power distribution network, which includes power and ground rails that supply voltage to the logic cells in the design.

Check PDN Creation: To confirm whether the PDN has been generated, you can check the current DEF (Design Exchange Format) environment variable. Use the following command:

echo $::env(CURRENT_DEF)

This will display the current DEF file, which contains the details of the design, including the power distribution network.

After generating the PDN, the design should have the necessary power and ground networks properly routed, ensuring that the cells and blocks receive stable power and minimize voltage droops.

Importance of PDN:

1. Power Integrity: The PDN is crucial for ensuring that all the cells in the design receive sufficient power without voltage fluctuations or noise.

2. Post-CTS PDN Generation: It is generated after CTS because the timing and placement of cells are critical to ensure that the power network properly supplies all components without introducing any noise or power issues.

## Routing using TritonRoute

This process is typically divided into two stages: Global Routing and Detailed Routing, which are handled by different routing engines.

1. Global Routing

Purpose: The global routing stage focuses on determining an overall routing path for the connections between the pins of the design, considering the entire chip's layout.

Routing Engine: The FASTE ROUTE engine is responsible for global routing. It divides the routing region into a grid of cells, forming a coarse 3D routing graph to guide the detailed routing process. It generates an initial, approximate path for connections between the various components.

Outcome: The result of global routing is the identification of coarse routes between components, which guides the more detailed routing stage. It ensures that no two nets interfere with each other at this stage.

2. Detailed Routing

Purpose: Detailed routing refines the global routing paths by implementing actual physical connections between pins, ensuring that the routing satisfies design rules and constraints.

Routing Engine: The TritonRoute engine is responsible for detailed routing. It refines the coarse global route data by considering the layout's physical constraints and optimizing routing paths.

### Key Features of TritonRoute in Detailed Routing

1. Initial Detailed Routing:

TritonRoute starts the detailed routing by analyzing the global route and building upon it to create the actual routing of metal layers.

It ensures that all routes are physically feasible, adhering to rules such as wire width, spacing, and layer constraints.

2. Adherence to Pre-Processed Route Guides:

TritonRoute places heavy emphasis on following the guidance provided by the global routing stage.

Pre-Processed Route Guides are directional routing paths that TritonRoute uses to determine the optimal paths. These guides help maintain consistency and simplify the routing task.

### Key Actions Involving Route Guides:

Initial Route Guide Analysis: TritonRoute analyzes the pre-processed route guides. It handles cases where the guides are not directional by converting them into unit widths that can be utilized for routing.

Guide Splitting: If non-directional routing guides are encountered, TritonRoute splits these into unit widths. This ensures the design complies with design rules for routing layers.

Guide Merging: When adjacent guides are orthogonal or touching, TritonRoute merges them to streamline the routing process and optimize the available routing space.

Guide Bridging: When encountering parallel guides, TritonRoute bridges these with an additional layer to ensure the routing continues smoothly within the existing constraints.

Connectivity Requirements:

TritonRoute ensures that each unconnected terminal (or pin) of a standard cell instance is overlapped by a routing guide. In other words, each pin should align with its designated routing guide, represented as a black dot (pin) within a purple box (metal layer). This ensures that routing paths are properly connected to the cell’s pins.

Routing Optimization:

MILP-based Panel Routing: TritonRoute employs a Mixed Integer Linear Programming (MILP) based routing scheme, optimizing the routing paths.

Intra-layer and Inter-layer Routing: The routing framework uses both intra-layer (within the same metal layer) parallel routing and inter-layer (between metal layers) sequential routing. This combination helps achieve optimal paths by reducing congestion and improving signal integrity.

### Step-by-Step Routing Process

1. Ensure PDN is Set:

Before starting the routing process, make sure that the CURRENT_DEF environment variable is set to pdn.def. This is critical because it tells the system that the PDN is ready and will be used in the subsequent routing process.

echo $::env(CURRENT_DEF)  # Ensure it is set to pdn.def

2. Start Routing:

To begin the routing process, use the following command in the terminal:

run_routing

3. Configuring Routing Options:

The routing options, including routing strategies, are specified in the config.tcl file. This file allows the designer to customize various aspects of the routing process.

You can also optimize routing by selecting different versions of the TritonRoute Engine. However, it's important to note that there is a trade-off between optimizing the route and the runtime of the routing process.

Optimized routes generally lead to better signal integrity and reduced congestion but take longer to compute.

Faster routing sacrifices some optimization for quicker runtime.

4. Runtime Considerations:

For example, with the current version of TritonRoute, routing for the picorv32a design may take approximately 30 minutes. The actual runtime depends on the complexity of the design and the routing strategy chosen.

5. Check for Routing Completion:

Once the routing process is completed, the system will produce the routing results. At this stage, the design should be free of any Design Rule Check (DRC) violations.

The expected outcome after successful routing is a zero DRC violation, indicating that the routing is compliant with the design rules and no errors have been detected.

You can verify this by checking the final DRC report or inspecting the layout visually.

![image](https://github.com/user-attachments/assets/220cef68-15a6-4086-8185-2a6546daa591)

Also the tns and wns can be seen using the comands (report_checks, report_skew,etc)

![image](https://github.com/user-attachments/assets/f063d716-f7ca-4f5e-a30d-6c479ee66ea0)

Next steps in the physical design include sign off checks (LEC,PV check, STA signoff etc).
