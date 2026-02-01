# Theory:
In the theory session, we were first introduced to a sample QFN-48 package and discussed the various components present inside a packaged chip. The main components of a chip include:

1.Pads: Pads are used to establish electrical connections between the integrated circuit (IC) and the external package. They enable the transmission of electrical signals into and out of the chip.

2.Core: The core is the central part of the chip where all the digital logic is implemented. It contains essential components such as logic gates, multiplexers, and flip-flops that perform the processor’s functionality.

3.Die: The die is the silicon area that houses the core. A single die can contain one or multiple cores.

We also learned about foundry IPs, which are pre-designed and pre-verified components provided by the foundry or designer. These IPs are extensively tested and commonly include blocks such as PLLs, ADCs, SRAMs, and other standard components.

In addition, macros were introduced. Macros are similar to foundry IPs but consist purely of digital logic and are pre-built for common use cases. Examples include GPIO and SPI blocks.
The session then moved on to an introduction to OPENLANE, covering its workflow, file structure, and the various tools it integrates.
OPENLANE is an open-source, automated RTL-to-GDSII flow that enables chip design with minimal human intervention. It leverages freely available tools such as Yosys, OpenROAD, Magic, and others to convert RTL designs into GDSII layouts.

We were also taught the complete RTL-to-GDSII design flow, which includes the following stages:

1.Synthesis: The Verilog RTL is synthesized to generate a gate-level netlist. Static Timing Analysis (STA) is performed on this netlist before proceeding to floorplanning.

2.Floorplanning and Power Distribution: Based on the synthesized netlist, the layout of the core is planned to efficiently place standard cells. During power planning, power and ground networks are created across the chip to ensure reliable power delivery.

3.Placement: After floorplanning and power planning, cells are placed within the core. This step includes global placement, which provides an approximate cell arrangement, and detailed placement, which refines cell positions for correctness and efficiency.

4.Clock Tree Synthesis (CTS): Once placement is complete, the clock network is generated to distribute the clock signal to all required cells. Structures such as H-trees or X-trees may be used to minimize clock skew and jitter.

5.Routing: In this stage, electrical connections between components are created using metal interconnects. Multiple metal layers are used to avoid wire overlap and routing conflicts.

6.GDSII Generation: Before finalizing the design, Design Rule Checks (DRC) are performed to identify and fix any violations. After successful verification, the design is exported as a GDSII file, which the foundry uses for chip fabrication.

Throughout this entire flow, various tools are employed, including Yosys for synthesis, ABC for technology mapping, IOPlacer for input/output placement, PDN for power distribution, RePlAce for global placement, TritonCTS for clock tree synthesis, and Magic for layout visualization and DRC checks.
# Lab:
Here in the first lab we though about the basics on how to run the OPENLANE what are the different files which would be required to us for the geration from the RTL to GDS II, we were being though about the PDK(Process Design Kits), about the lef files, def files, spef files and where would the results be present where would these all the information files such as the lef, def, spef would be present and how to run all of them and finally we were being taught how to calculate the total flop ratio after doing the synthesis part.

![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/aee2d2f6ca888e33e317ac4cfb985c3e76271c00/Day1/Screenshot%20(22).png)
