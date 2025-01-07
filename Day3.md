# Day -3 Design Library Cell using magic layout and ngspice charcterization

### IO Placer Revision in OpenLANE
In OpenLANE, you have the flexibility to adjust IO Placement configurations during the physical design flow. The default setting for IO placement is typically random equidistant, but this can be changed on the fly to suit the specific design requirements. To alter the IO placement mode, you can use the following command after running the floorplan step:

bash$ set ::env(FP_IO_MODE) 2

This command switches the IO placement to mode 2, which places the input-output pins based on a different algorithm, often stacking them one over the other rather than maintaining equidistant spacing. It utilizes a technique such as the Hungarian algorithm, which aims to optimize pin placement based on various factors like signal timing, congestion, and layout constraints.

Important Note: Modifying the IO mode using this method only affects the current session. It does not change the configuration in the runs/config.tcl file, meaning the change is temporary unless it is explicitly saved in the configuration for future runs.

### SPICE Deck Creation for CMOS Inverter
The SPICE deck is a file that defines the circuit connectivity, component values, and simulation parameters. When designing a CMOS inverter, the PMOS and NMOS transistor components are typically defined in the netlist with specific parameters such as channel width (W) and length (L). For example:

" M1 OUT IN VDD VDD PMOS W=.375U L=.25u "

In this example:

M1 is the component name.

OUT, IN, VDD are the drain, gate, and source nodes respectively.

The transistor type is PMOS with a width of 0.375µm and length of 0.25µm.

The width of the PMOS transistor is typically designed to be 2 to 3 times that of the NMOS to compensate for the slower hole mobility in PMOS transistors, ensuring balanced rise and fall times.

![image](https://github.com/user-attachments/assets/2d1de124-dc48-41e8-9e30-70a18a9ab2b9)

### SPICE Simulation for CMOS Inverter
To simulate a CMOS inverter, NGSPICE is used as the simulator. The simulation starts with an operating point analysis (.op), where the input voltage (Vin) is swept across a range, typically from 0V to 2.5V, with a small step size (e.g., 0.05V).

Steps for SPICE Simulation:

1. Open NGSPICE Simulator: Launch the simulator on your system.

2. Source the Circuit File: Use the source command to load the SPICE deck containing the CMOS inverter circuit.

3. Execute the Simulation: Run the simulation with the run command.

4. Set Plot: Use the setplot command to configure the plot options and select the type of simulation to run.

5. Display Results: The display command allows you to choose which nodes to plot. Typically, you'll plot the input voltage (Vin) vs output voltage (Vout) to analyze the behavior of the CMOS inverter.

![image](https://github.com/user-attachments/assets/de9e0dfb-a678-4fb7-8d49-21569d677465)

###  Threshold Voltage

In CMOS (Complementary Metal-Oxide-Semiconductor) technology, the switching threshold (Vm) is one of the most important parameters for determining the transition point between the logical "0" and "1" in a CMOS inverter. Graphically, Vm corresponds to the point where the input voltage (Vin) is equal to the output voltage (Vout), which typically occurs at the middle of the inverter’s DC transfer characteristic curve. 
Understanding the Graphical Behavior of a CMOS Inverter

A CMOS inverter’s DC transfer characteristic graph is a plot of the output voltage (Vout) versus the input voltage (Vin). The typical curve for a CMOS inverter is an S-shaped curve, showing the following key features:

When Vin is low (0V):

The NMOS transistor is turned off, and the PMOS transistor is turned on.

The output voltage (Vout) is high (VDD).

When Vin is high (VDD):

The PMOS transistor is turned off, and the NMOS transistor is turned on.

The output voltage (Vout) is low (0V).

Transition Region:

In the middle of the curve, where Vin changes from low to high (0 to VDD), there is a region of transition. Here, both PMOS and NMOS transistors are partially turned on, and the output voltage (Vout) is not fully high or low. This region is crucial for defining the switching threshold (Vm).

Graphical Representation of Switching Threshold (Vm)

The switching threshold (Vm) occurs at the point on the curve where the output voltage (Vout) is equal to the input voltage (Vin). This is typically at the midpoint of the transition region. In an ideal CMOS inverter, the Vm is usually around VDD/2. However, it can vary slightly due to factors like process variation, temperature, and the specific characteristics of the PMOS and NMOS transistors.

Vm is the point on the graph where:

𝑉in=Vout=𝑉𝑚

![image](https://github.com/user-attachments/assets/8aed983f-395f-47ab-bb7e-c3a0688c5248)

This means that at this point, the inverter is in a balanced state, and both the PMOS and NMOS transistors are conducting.

Impact of Properly Calibrating Vm:

The proper calibration of the switching threshold (Vm) is critical for ensuring that the inverter works correctly. If Vm is too high or too low, it can lead to several undesirable effects:

Excessive Current Leakage: If the threshold is not properly set, both transistors (PMOS and NMOS) may be partially turned on simultaneously, leading to current leakage between VDD and GND. This is known as a short-circuit current, which increases power consumption unnecessarily and can cause overheating in the circuit.

Delays in Switching: Incorrect Vm values can lead to slower transitions between the "0" and "1" logic states. A misaligned Vm could cause an inverter to switch more slowly, affecting the overall speed of the digital circuit, especially in high-frequency designs.

Error Prone Transitions: A Vm that is not calibrated correctly can result in unreliable transitions, where the inverter may not reliably change states. This increases the chance of errors in logic circuits, especially in noise-prone or high-speed environments.

Graphical Consideration for Robustness:

The graph of a CMOS inverter also provides insight into its robustness. The steepness of the transition between the low and high states is important because a steeper curve indicates that the inverter switches quickly with a small change in input voltage, leading to better performance and lower susceptibility to noise.

Steep Transition: A steep S-curve implies fast switching, ensuring that the inverter quickly responds to input changes, minimizing delay.

Shallow Transition: A shallow curve, on the other hand, could mean slower switching, which can lead to increased delays and higher power consumption.

Switching Threshold and Noise Margins:

Another important aspect of the switching threshold (Vm) is its relationship to noise margins. The noise margin refers to the inverter's ability to tolerate noise or fluctuations in the input signal without causing errors. A well-defined Vm allows the inverter to maintain a sufficient noise margin between the logic high and logic low levels, ensuring reliable operation even in noisy environments.

### Static and Dynamic Simulation of CMOS Inverter

In the context of CMOS inverter design and analysis, static and dynamic simulations are essential for understanding the behavior of the inverter and determining key parameters like the switching threshold (Vm), propagation delay, and transition times.

1. Static Simulation (DC Transfer Analysis)

The DC Transfer Analysis is used to determine the switching threshold (Vm) of the CMOS inverter. During static simulation, the input voltage (Vin) is gradually increased from 0V to the supply voltage (VDD, typically 2.5V), and the corresponding output voltage (Vout) is observed. This analysis is done at a constant DC state, which means there are no time-varying signals; the inverter is analyzed under steady-state conditions.

Step-by-step simulation: The simulation involves sweeping the input voltage (Vin) from 0V to 2.5V (or the designated VDD), taking small incremental steps (such as 0.05V per step). For each input voltage value, the output voltage (Vout) is computed, and a plot of Vout vs Vin is generated.

This plot is the DC Transfer Curve, where:

The horizontal axis represents the input voltage (Vin).

The vertical axis represents the output voltage (Vout).

The curve is S-shaped, with Vin ranging from 0V to VDD and Vout switching from VDD to 0V, with the switching threshold (Vm) occurring around the point where Vin = Vout. The Vm value is typically around VDD/2 for ideal inverters, but it may shift based on the characteristics of the PMOS and NMOS transistors.

![image](https://github.com/user-attachments/assets/e7ab1327-1fb3-437a-a2e2-157dab3674f8)

Key observations in static simulation:

Switching Threshold (Vm): The point where the inverter changes its output logic level from high to low (or vice versa). This is the critical point in the S-curve.

Noise Margin: The region around the switching threshold defines the noise margin, which is a measure of how much noise can be tolerated on the input without causing errors in the output.

2. Dynamic Simulation (Transient Analysis)

In dynamic simulation, we analyze the behavior of the CMOS inverter in response to time-varying inputs, typically involving pulses or square wave inputs. This simulation helps to determine propagation delay and transition times (rise and fall times) during the inverter’s switching operation.

Step-by-step simulation: The input voltage (Vin) is typically applied as a square wave or pulse signal that varies with time. In this type of simulation, we are interested in how the inverter responds to rapid changes in input, especially when the input signal is switching from low to high or vice versa.

Propagation delay: This is the time it takes for the output to transition from one logic state to another after the input has crossed a threshold (either from low to high or high to low). This is measured as the time between when the input crosses 50% of its transition and when the output reaches 50% of its corresponding transition.

Rise and fall time: These are the times it takes for the output to transition from 10% to 90% (rise) or from 90% to 10% (fall) of the supply voltage (VDD). These times are crucial for determining the speed at which the CMOS inverter can switch.

![image](https://github.com/user-attachments/assets/06c3f2a0-2bb6-45d6-8da6-b0d6b0a442ce)

SPICE NETLIST:

![image](https://github.com/user-attachments/assets/169a2752-2e16-4dbc-bf47-003bd4ca309c)

Key observations in dynamic simulation:

Rise Transition Time: The time it takes for the output to go from 10% to 90% of its final high state (VDD).

Fall Transition Time: The time it takes for the output to go from 90% to 10% of its final low state (0V).

Propagation Delay: The time delay from when the input signal reaches 50% of its final value to when the output reaches 50% of its final value.

### Steps to clone and view Inverter Layout by VLSI System Design

1. Clone the Repository: First, clone the repository containing the custom inverter design files by running the following command in your terminal:

 " git clone https://github.com/nickson-jose/vsdstdcelldesign "

This will create a directory called vsdstdcelldesign in your working directory.

2. Copy the Technology File: Next, copy the appropriate technology file (sky130A.tech) into the cloned repository directory. This file is necessary for working with the layout in MAGIC, the VLSI design tool. Use the following command:

"cp /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech /home/vsduser/Desktop/work/tools/openlane_working_dir/openlane/vsdstdcelldesign/"

This command copies the sky130A.tech file to the vsdstdcelldesign directory where the inverter design files are located.

3. Open the Inverter Layout in MAGIC: Finally, to visualize the layout of the custom inverter, use the MAGIC VLSI tool. Run the following command to open the inverter layout file (sky130_inv.mag) in MAGIC:

"magic -T sky130A.tech sky130_inv.mag & "

This command will launch MAGIC with the specified technology file (sky130A.tech), and it will load the inverter layout (sky130_inv.mag). The & allows the process to run in the background, so you can continue using the terminal for other tasks.

![image](https://github.com/user-attachments/assets/540d9ba5-e841-4248-a0f7-5e322bae4d8a)

### 16-Mask CMOS Fabrication Process

The 16-mask CMOS fabrication process involves several critical steps to create the active regions, well formations, gate terminals, and interconnects for a CMOS-based integrated circuit. Below is a breakdown of the major steps involved in this process:

1. Create Active Regions
   
The process starts with the selection of a substrate, typically a P-doped Silicon Substrate. The substrate should be less doped than the wells.

Silicon Dioxide Growth: A 40nm layer of Silicon Dioxide (SiO2) is grown.

Silicon Nitride Deposition: An 80nm layer of Silicon Nitride (Si3N4) is deposited on top.

Photoresist Layer Deposition: A photoresist layer is applied to protect specific regions.

Mask-1 Layer: Mask-1 is deposited over the photoresist layer to protect the transistor active regions during the etching process.

UV Light Exposure: Ultraviolet (UV) light is used to expose and remove the photoresist layer in unmasked areas.

Mask-1 Removal: After UV exposure, the mask-1 and photoresist layers are removed.

Oxide Growth: The wafer is placed in a furnace to grow oxide in unprotected areas.

Si3N4 Removal: The Si3N4 layer is removed using hot phosphoric acid, leaving behind the p-substrate and SiO2.

2. Formation of N and P Wells

Mask-2 and Mask-3 Layers: These masks are used to protect specific regions during the formation of the N-well and P-well.

Mask-2 protects the N-Well (PMOS side) while the P-Well (NMOS side) is being fabricated.

Mask-3 protects the P-Well while the N-Well is being formed.

UV Light Exposure: UV light is applied to remove the exposed photoresist.

Well Formation: The chip is placed in a furnace for a process called the Twintub Process, where boron (B) is used for P-well formation, and phosphorus (P) is used for N-well formation.

3. Formation of Gate Terminal

Gate Formation: The gate terminal is crucial for controlling the threshold voltage of the CMOS.

Mask-4: A photoresist layer is applied and mask-4 is used to define areas where implantation of low-energy boron occurs at the surface of the p-well to control the threshold voltage.

Mask-5: Similarly, phosphorous or arsenic is implanted into the n-well using mask-5.

Oxide Regrowth: The damaged oxide layer due to implantation is fixed by removing excess SiO2 using hydrofluoric acid and regrowing a high-quality SiO2 on the p-substrate.

Polysilicon Deposition: A polysilicon film is added to form the gate.

Mask-6: A final mask-6 is used for photolithography and etching to shape the gate terminal.

4. Lightly Doped Drain (LDD) Formation

LDD Formation: Lightly Doped Drain (LDD) regions are formed to mitigate the hot electron effect and short channel effects.

Mask-7 (NMOS) and Mask-8 (PMOS): Masks are used for lightly doping the N-type and P-type regions, respectively.

Spacer Formation: Silicon dioxide is used to form spacers that protect the lightly doped regions, and anisotropic plasma etching is applied.

High-Temperature Annealing: This process is done to activate the dopants and repair the implantation damage.

5. Source and Drain Formation

Screen Oxide: A thin screen oxide is added to avoid channeling, which is the issue when implantations penetrate too deeply into the substrate.

Mask-9 (N+ Implantation) and Mask-10 (P+ Implantation): These masks are used to form the source and drain regions by implanting N+ for NMOS and P+ for PMOS.

Sidewall Spacers: The spacers formed in the previous steps maintain the correct spacing during N+/P+ implantation.

6. Local Interconnect Formation

Screen Oxide Removal: The thin screen oxide is removed to open up the source, drain, and gate regions for contact building.


Titanium Deposition: Titanium is used for the interconnects due to its low resistance.

Titanium Nitride (TiN) Etching: Titanium Nitride (TiN) is etched using an RCA cleaning process to create the first-level local contact.

7. Higher-Level Metal Formation

Doped Silicon Dioxide Deposition: A layer of phosphosilicate or borophosphosilicate glass (SiO2 doped with phosphorous or boron) is deposited to planarize the surface.

Chemical Mechanical Polishing (CMP): The surface is polished to ensure uniformity.

Contact Hole Formation: Contact holes are created using photolithography to provide access for the metal layers.

Mask-12, Mask-13, Mask-14, Mask-15, Mask-16:

Mask-12 creates the first contact holes.

Mask-13 is used to create the first aluminum layer for contact.

Mask-14 and Mask-15 create the second contact holes and second aluminum layer.

Finally, Mask-16 is used for making contacts to the topmost metal layer.

Conclusion:
This 16-mask CMOS fabrication process allows for the precise formation of transistors, wells, gates, drains, and interconnects. The masks and photolithography play a critical role in defining the regions and ensuring accurate doping and implantation. The process also focuses on addressing potential issues like hot electron effects, short-channel effects, and maintaining the integrity of the oxide layers. The final outcome is a fully functional CMOS integrated circuit ready for higher-level metal layers and packaging.

![image](https://github.com/user-attachments/assets/89c96e25-f646-49f7-94cc-ec18882135f9)

### SKY130 Basic Layer Layout and LEF Using CMOS Inverter

In the SKY130 process, the layers are specifically organized to ensure precise placement and connection of components. Below is a description of the basic layout and LEF (Library Exchange Format) for a CMOS inverter using the SKY130 process.

#### CMOS Inverter Layout Overview

A CMOS inverter consists of two key components:

PMOS (P-type Metal-Oxide-Semiconductor) transistor

NMOS (N-type Metal-Oxide-Semiconductor) transistor

These two transistors are connected as follows:

Gates of PMOS and NMOS: The gates are connected together and fed the same input signal (denoted as A).

Source of NMOS: Connected to the ground (denoted as VGND).

Source of PMOS: Connected to the power supply (denoted as VPWR).

Drains of PMOS and NMOS: The drains are connected together and provide the output signal (denoted as Y).

SKY130 Layer Structure for CMOS Inverter

The layout of the CMOS inverter in the SKY130 process involves several key layers, including local interconnect layers and metal layers.

Local Interconnect (locali) Layer:

The first layer in the SKY130 process is the local interconnect layer. This layer is used to connect components at the lowest level. In a CMOS inverter, the local interconnect layer is crucial for connecting the drain of the PMOS to the drain of the NMOS, as well as the gate terminals of both transistors.

Metal 1 Layer :

Metal 1 is the first metal layer above the local interconnect. It is represented in purple color. This layer is often used for connecting components within the same region of the layout, such as connecting the source of the PMOS to VDD and the source of the NMOS to GND.

Metal 2 Layer :

Metal 2 is another metal layer above Metal 1, represented in pink color. Metal 2 is typically used for routing connections that need to span across larger areas, especially when multiple components need to be connected over a longer distance. It connects the output (Y) to other parts of the circuit.

#### Viewing Connections in the Layout

To inspect the connections between components in the layout:

Hover Over Connections: Place the cursor over a specific area where components are connected, such as the drain or gate of a transistor.

Press 'S': After placing the cursor over the area, press the 'S' key once. Then type 'what' in the Tkcon window. This will display the component name and provide details about the connection in the Tkcon window, helping you identify how the parts are interconnected.

![image](https://github.com/user-attachments/assets/93b59762-3faa-4161-9bb8-b0628a4e7c1c)

![image](https://github.com/user-attachments/assets/623b1ce8-d54f-4b28-b3c7-f749bfae1ac6)

#### LEF (Library Exchange Format) for CMOS Inverter

LEF is a file format used to describe the physical design of standard cells, including transistors, metal layers, and other components. In the context of the SKY130 CMOS inverter, the LEF file would describe the following:

Dimensions of the Inverter: The layout dimensions for both PMOS and NMOS transistors, as well as the overall dimensions of the CMOS inverter.

Layer Information: Specifications for each layer, including local interconnect, metal 1, and metal 2, along with their respective properties (e.g., width, height, and spacing).

Pins and Ports: The pins representing the input (A), output (Y), and power/ground connections (VPWR, VGND). The LEF file would also define the placement of these pins in the layout.

The LEF file will describe the following attributes:

Layer definitions: Describes the metal and interconnect layers used in the design.

Cell Dimensions: Defines the cell dimensions, including the height and width of the standard cell.

Pins: Specifies the placement of the input, output, and power/ground pins.

Antenna Effects: Provides information on potential antenna effects that could occur due to large metal layers covering smaller transistors.

#### Generating SPICE Netlist of the custom inverter layout 

To extract the SPICE netlist with parasitic information for the custom inverter layout in MAGIC, follow these steps:

Extract the layout information:

After loading the layout in MAGIC, open the Tkcon (command window).

Enter the following command to extract all the layout components: "extract all"

This command will extract all the geometry and connectivity information from the layout.

Set extraction thresholds for parasitic extraction:

To control the accuracy of the extraction, you can set thresholds for capacitance (cthresh) and resistance (rthresh) to filter out small parasitic values that are not significant for simulation. Use the following command:

"ext2spice cthresh 0 rthresh 0"

Here, cthresh 0 and rthresh 0 specify that no thresholding is applied (all capacitances and resistances will be included in the extraction). You can adjust these thresholds if you want to filter out small parasitics.

Perform the SPICE extraction:

Now, run the actual extraction to generate the SPICE netlist with the parasitics included: "ext2spice"

This will generate a SPICE netlist that includes all the extracted components, as well as the parasitic capacitances and resistances. This netlist can then be used for simulation.

These commands ensure that you can perform a detailed SPICE extraction of the custom inverter layout, including all the parasitic components, which is crucial for accurate simulation and analysis of the circuit's behavior.

This should have generated a file ending with .spice extension, which itself is the spice netlist

![image](https://github.com/user-attachments/assets/07f7be74-8501-417c-b9ba-ebd90e340fd8)


open the netlist using gvim editor: 

![image](https://github.com/user-attachments/assets/589da4c0-e6da-4be0-8006-e86e1330782b)

This default netlist can now be modified to get a transient response as shown below: 

![image](https://github.com/user-attachments/assets/b8fdecf3-44b7-4cae-ba59-a6d5a8fd1ded)

To load the SPICE file for simulation in NGSPICE, type the following command : "ngspice sky130A_inv.spice" and to plot the transient response type the command "plot y vs time a".

![image](https://github.com/user-attachments/assets/b37046bb-69e5-4c7b-b4e8-3d0fcd6f7a47)

To characterize a standard cell (such as a CMOS inverter) using key parameters—rise transition, fall transition, rise cell delay, and fall cell delay—you can follow these general steps based on waveform analysis from SPICE simulations:

1. Rise Transition

The rise transition is the time taken for the output waveform to transition from 20% to 80% of the maximum output voltage (VDD).

To calculate the rise transition:

Identify the times when the output reaches 20% and 80% of VDD.

The difference between the time at 80% and 20% gives the rise transition time.

Formula:

Rise Transition = Time at 80% of VDD - Time at 20% of VDD

2. Fall Transition

The fall transition is the time taken for the output waveform to transition from 80% to 20% of the maximum output voltage (VDD).

To calculate the fall transition:

Identify the times when the output reaches 80% and 20% of VDD during the falling edge of the signal.

The difference between the time at 20% and 80% gives the fall transition time.

Formula:

Fall Transition = Time at 20% of VDD - Time at 80% of VDD

3. Rise Cell Delay

The rise cell delay is the time difference between when the input signal reaches 50% of its value (rising edge) and when the output signal reaches 50% of its value (rising edge).

To calculate the rise cell delay:

Identify the times when the input and output signals each reach 50% of their respective values during the rising edge of the signal.

The difference between the time at 50% input and the time at 50% output gives the rise cell delay.

Formula:

Rise Cell Delay = Time at 50% input - Time at 50% output

4. Fall Cell Delay

The fall cell delay is the time difference between when the input signal reaches 50% of its value (falling edge) and when the output signal reaches 50% of its value (falling edge).

To calculate the fall cell delay:

Identify the times when the input and output signals each reach 50% of their respective values during the falling edge of the signal.

The difference between the time at 50% input and the time at 50% output gives the fall cell delay.

Formula:

Fall Cell Delay = Time at 50% input - Time at 50% output

![image](https://github.com/user-attachments/assets/9dbfac5e-223d-4cf9-961f-b091a9c6e949)

Summary:

Rise Transition: The time it takes for the output to transition from 20% to 80% of VDD.

Fall Transition: The time it takes for the output to transition from 80% to 20% of VDD.

Rise Cell Delay: The time difference between the 50% rise of the input and the 50% rise of the output.

Fall Cell Delay: The time difference between the 50% fall of the input and the 50% fall of the output.

### Introduction to SKY130 PDK and Setup to View Layouts

The SKY130 PDK (Process Design Kit) is a set of files that define the characteristics and manufacturing rules for the SKY130 technology node. These files are essential for designing and verifying custom integrated circuits in this process. The technology file (tech file) is a key component of the PDK and is required by layout tools such as Magic to perform various operations like layout creation, extraction, and viewing.

Steps to Download and Set Up Labs for Viewing Layouts

Here is the process to download the required files and set up the environment to view and work with the layouts:

Download the PDK Package: First, you need to download the package containing the required files for the lab exercise and Magic layout. You can do this using the wget command, which downloads files from the internet non-interactively.

"wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz"

This will download the drc_tests.tgz file, which contains the necessary files for Magic, including the layout files and other resources required for the lab exercises.

Extract the Tarball: After the download is complete, you need to extract the tarball to get access to the files inside. You can do this using the following command:

"tar -xzvf drc_tests.tgz"

This will extract the contents of the drc_tests.tgz file into a directory named drc_tests.


Open Magic tool using the command :

"magic -d XR"

The -d XR option specifies that Magic should use the X11 display (for graphical output). You can adjust this depending on your setup.

Then, in the empty magic prompt, click "FILE", which is at the top and select open and then met3.mag under that, to obtain this screen view wherein we can see a number of independent layouts contain certain DRC errors

![image](https://github.com/user-attachments/assets/837ac197-093f-4c30-8214-d460e9baedab)

### Lab Excercise to Fix poly.9 error in SKY130 Tech-File

In the Tckon gui, type the command:  "load poly"

![image](https://github.com/user-attachments/assets/3ac5d164-28e1-4e19-82a3-7f16252005b1)

Now, to focus on the poly 9 rule as explained at this link (https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html#rules-periphery--page-root) which states that poly resistor spacing to poly or spacing (no overlap) to diff/tap should be atleast 0.48um! Now, we can look at the below screenshot to find that this is not true, here and hence must be fixed by changing the tech file to include this DRC -:

Adding Rules to the sky130A.tech File for Poly Resistor Spacing

To define spacing rules for the p-poly resistor with poly non-resistor and the n-poly resistor with poly non-resistor in the sky130A.tech file, follow these steps:

1. Locate and Open the sky130A.tech File:

Navigate to the directory where the sky130A.tech file is located.

Open the file using a text editor. For example, you can use gvim or nano:

gvim drc_tests/sky130A.tech

2. Understand Existing Rules:

The rules for poly.9 already define the spacing between the n-poly resistor and the p-poly resistor with diffusion.

Additional rules are required for spacing between the p-poly resistor with poly non-resistor and the n-poly resistor with poly non-resistor.

3. Add New Rules:
Scroll to the section of the file where spacing rules are defined.

Add the following highlighted rules under the appropriate section, ensuring they align with the file's format and syntax.

![image](https://github.com/user-attachments/assets/dda4eaa5-b4c8-4ffc-9ab4-efb0f5fecf1f)

since the tech file has been updated, now reload the tech file into magic using the command " tech load sky130A.tech " and then type "check drc". 

Therefore the error can be fixed by updating the tech file :

![image](https://github.com/user-attachments/assets/2a0c9e90-fb92-4afa-b669-d35529277cab)


{IMAGE CREDITS : All the images used are from the lecture provided in the course or from the author's system screenshots}
