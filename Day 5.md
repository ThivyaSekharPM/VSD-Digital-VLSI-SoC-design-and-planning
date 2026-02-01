# Theory

## 1. Overview of Maze Routing and Lee’s Algorithm

Routing refers to the process of creating the most efficient physical connections between components in an integrated circuit, such as clocks, flip-flops, and standard cells.

Several routing techniques are commonly used, including:

- Steiner Tree Algorithm

- Line Search Algorithm

- Lee’s Maze Routing Algorithm (primary focus here)

The key goals of routing are to:

Determine the shortest possible connection path

Reduce unnecessary bends and detours

Adhere to Manhattan routing style, allowing only horizontal and vertical paths

From a software perspective, routing involves computing an optimal path between points. In VLSI physical design, this translates into laying out metal interconnects on silicon to ensure reliable signal transmission. Lee’s Algorithm is especially suitable for grid-based routing scenarios commonly found in chip design.

#2. Working Principle of Lee’s Algorithm

Lee’s Algorithm operates on a grid-based representation of the routing area. It systematically explores all valid paths between a source and a destination to guarantee identification of the shortest route.

#Step 1: Grid Setup

The routing grid is initialized with the following cell types:

- Obstacles: Blocked regions such as macros or reserved areas

- Empty Cells: Available routing spaces

- Source: Starting point of the connection

- Target: Endpoint of the route

#Step 2: Wave Propagation

Starting from the source, the algorithm expands in all four cardinal directions—up, down, left, and right. Each expansion step increases the cost value, and this wave continues until the target is reached or no further expansion is possible.

#Step 3: Path Backtracking

Once the target is encountered, the algorithm traces back to the source by following cells with decreasing cost values, forming the shortest valid path.

Important constraints include:

- Only vertical and horizontal movements are permitted

- Blocked regions must be avoided

- Diagonal paths and overlaps are not allowed

#3. Design Rule Checking (DRC)

All routing must comply with fabrication rules defined by the foundry, collectively known as Design Rules.

Typical DRC Requirements:

- Minimum allowable wire width

- Minimum spacing between adjacent wires

- Minimum pitch between routing tracks

Proper via dimensions and spacing between layers

Purpose of DRC:

- Ensure the design can be manufactured correctly

- Prevent shorts and electrical faults

- Maintain signal integrity and reliability

Any DRC violations identified after routing must be resolved before the design can proceed to fabrication.

#4. Signal Shorting and Metal Layer Transitions

Signal shorts are a common routing issue and can cause:

- Functional malfunctions

- Timing problems such as setup or hold violations

Mitigation Approach:

- To resolve congestion or conflicts, routes may be moved to higher metal layers using vias. These vias must also follow strict design rules, including:

- Minimum via width

- Minimum via spacing

- Preferred routing directions for each metal layer (e.g., M1 vertical, M2 horizontal)

Higher metal layers often require wider wires, which adds complexity but helps alleviate routing congestion.

5. Stages of Routing in VLSI Design

Routing is generally carried out in two main stages:

Global Routing

Also known as coarse or fast routing

Divides the chip into larger routing regions

Defines an initial routing plan and identifies available routing channels

Detailed Routing

Performs precise routing at the track level

Completes all signal connections

Ensures full compliance with DRC and design constraints

Global routing establishes the overall plan, while detailed routing refines and finalizes the layout.

6. TritonRoute: Introduction and Capabilities

TritonRoute is an open-source detailed routing engine integrated into the OpenROAD flow and is typically executed using the run_routing command.

Key Capabilities:

Routing Guide Compliance

Follows routing guides generated in earlier stages

Respects preferred routing directions for each metal layer

Utilizes upper metal layers when non-preferred routing is required

Inter-Guide Connectivity

Divides the routing space into vertical panels

Ensures legal and continuous routing between panels through proper bridging

Layer-Aware Routing Strategy

Routes even-numbered panels first, followed by odd-numbered panels

Improves routing efficiency and reduces conflicts

7. Optimization Using MILP

TritonRoute applies Mixed Integer Linear Programming (MILP) to optimize routing between Access Point Clusters (APCs).

Optimization Process:

Evaluate the cost associated with each access point

Build a Minimum Spanning Tree (MST)

Select the lowest-cost routing paths

This approach minimizes wire length and congestion while maintaining connectivity.

8. Routing Execution and Final Output

When the run_routing command is executed, both global and detailed routing are performed.

Key Highlights:

Uses an iterative refinement strategy (Strategy 0)

Initially high DRC violations (e.g., ~25,000) are gradually reduced to zero

The routing process may take around 20–30 minutes, depending on design complexity

Multiple iterations (such as 30+ passes) are carried out to fully clean the layout

The final, verified design is generated in DEF or GDS format, making it ready for signoff and fabrication.

# Lab:

![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(194).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(195).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(196).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(197).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(198).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(199).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(200).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(201).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(202).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(203).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(204).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(205).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(206).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(207).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(208).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(209).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(210).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(211).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(212).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(213).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(214).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(215).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(216).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(217).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(218).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(219).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(220).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/0d83377eab591a1c5513b410e94986e4a8748c96/Day5/Screenshot%20(221).png)
