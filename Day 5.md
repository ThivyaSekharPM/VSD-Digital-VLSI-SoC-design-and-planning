# Theory

## 1. Introduction to Maze Routing and Lee’s Algorithm

Routing is the process of determining the most efficient physical connection between two points in an integrated circuit, such as between clocks, flip-flops, or other standard cells.

Several algorithms have been developed for routing, including:

- Steiner Tree Algorithm  
- Line Search Algorithm  
- Lee’s Maze Routing Algorithm (focus of this section)

The main objectives of routing are to:

- Identify the shortest, most efficient path  
- Minimize bends or zig-zags in the route  
- Follow Manhattan-style geometry (only vertical and horizontal lines)

In software terms, routing involves algorithmically finding a path between two or more points. In the physical design domain, it translates to placing metal wires on silicon to ensure correct signal transmission.

Lee’s Algorithm is particularly effective for grid-based routing problems, such as those encountered in VLSI physical design.

---

## 2. Lee’s Algorithm: Methodology

Lee’s Algorithm operates on a grid (or routing matrix) that represents the routing area. It systematically explores all paths from a source to a target node, ensuring the shortest path is identified.

### Step 1: Initialization

A grid is constructed and each cell is categorized as follows:

- Obstacle: Blocked regions such as macros or reserved spaces  
- Empty: Free routing space  
- Source: Starting point for the path  
- Target: Destination point

### Step 2: Wave Expansion

The algorithm starts at the source and expands outward in all four cardinal directions (up, down, left, right). Each step increments a cost value from the previous cell. This process continues until the target is reached or no further expansion is possible.

### Step 3: Backtracking

Once the target is reached, the algorithm backtracks from the target to the source by following decreasing cost values. This path represents the shortest route.

Key constraints:

- Only horizontal and vertical movements are allowed (Manhattan geometry)  
- Obstacles must be avoided  
- No diagonal paths or overlaps with blocked areas are permitted

---

## 3. Design Rule Check (DRC)

Routing must adhere to fabrication constraints defined by the foundry. These are known as Design Rules.

### Common DRC Constraints:

- Minimum wire width  
- Minimum spacing between adjacent wires  
- Minimum pitch (distance between similar routing layers)  
- Legal via usage between metal layers  

### Purpose of DRC:

- Ensure manufacturability of the chip  
- Prevent short circuits and electrical failures  
- Verify signal integrity and reliability

DRC violations must be cleaned post-routing to make the layout production-ready.

---

## 4. Signal Shorts and Layer Switching

A frequent issue in routing is signal shorting, which can lead to:

- Functional failures  
- Timing violations (setup or hold failures)

### Resolution:

Routes may be shifted to higher metal layers to avoid congestion or conflicts. This requires the insertion of vias. These vias must also adhere to:

- Minimum via width  
- Minimum via spacing  
- Routing direction constraints based on the metal layer (e.g., M1: vertical, M2: horizontal)

Higher layers may demand wider wires, increasing design complexity but helping resolve routing congestion.

---

## 5. Routing Phases in VLSI Design

Routing is typically divided into two major phases:

### Global Routing

- Also referred to as "Fast Routing"  
- Divides the routing area into larger tiles or grids  
- Establishes a preliminary routing plan, identifying routing channels between components  

### Detailed Routing

- Performs fine-grain routing at the track level  
- Finalizes all connections  
- Ensures compliance with DRC and design constraints  

Global routing sets up the routing plan, while detailed routing ensures its correctness and manufacturability.

---

## 6. TritonRoute: Overview and Features

TritonRoute is an open-source detailed routing engine used within the OpenROAD flow, typically invoked via the `run_routing` command.

### Key Features:

1. **Route Guide Awareness**  
   - Honors pre-defined routing guides from earlier stages  
   - Follows preferred routing directions (e.g., M1: vertical, M2: horizontal)  
   - Splits and re-aligns non-preferred directions using upper metal layers when needed  

2. **Inter-Guide Connectivity**  
   - Routing area is divided into vertical panels  
   - Ensures correct and legal routing between panels using bridging and preferred layer guidance  

3. **Layered Routing Strategy**  
   - Routes even-indexed panels first, then odd-indexed  
   - Promotes efficient intra- and inter-layer routing  

---

## 7. MILP-Based Optimization

TritonRoute uses Mixed Integer Linear Programming (MILP) for optimizing routing between Access Point Clusters (APCs).

### Steps:

- Calculate the cost for each potential access point  
- Construct a Minimum Spanning Tree (MST)  
- Identify and select the minimum-cost routing paths  

This ensures optimal wire usage and connectivity, reducing routing congestion.

---

## 8. Final Output and Routing Execution

When `run_routing` is executed, both global and detailed routing processes are performed.

### Key Details:

- Routing strategy: Strategy 0 (iterative refinement)  
- Initial DRC violations (e.g., 25,000) are progressively reduced to 0  
- May take approximately 20 to 30 minutes, depending on design complexity  
- Multiple iterations (e.g., 34) are performed to clean the layout  

The final routed design is output in DEF or GDS format, ready for signoff and fabrication.

---

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
