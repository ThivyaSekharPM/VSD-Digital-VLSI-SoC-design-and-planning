# Theory:
# Introduction to SPICE Deck for Circuit Simulation

The first step in running a SPICE simulation is to create a **SPICE deck**. This file contains all the necessary details to simulate an analog or digital circuit at the transistor level.

## Key Components of a SPICE Deck

### 1. Component Connectivity

Each component in the circuit must be connected using named **nodes**. These define how current flows through the circuit.

- **Example:**  
  For a transistor, connectivity includes:
  - Drain
  - Gate
  - Source
  - Substrate (bulk)

The **substrate connection** is crucial because it influences the **threshold voltage** of both NMOS and PMOS transistors.

### 2. Component Parameters

Each transistor must include:

- **W/L ratio**: Width-to-length ratio of the MOSFET channel.  
  These determine the current-driving strength of the transistors.

- **Example:**
  ```spice
  M1 out in Vss Vss nmos W=1u L=0.1u
  M2 out in Vdd Vdd pmos W=1u L=0.1u

# CMOS Fabrication Process – 16 Mask Flow
## 1. Active Region Formation

**Substrate:** Lightly P-doped silicon wafer.

**Isolation (LOCOS method):**
- Grow 40 nm of SiO₂.
- Deposit 80 nm of Si₃N₄.
- Apply photoresist and use Mask-1 to define active regions.
- Expose to UV light, remove unmasked areas.
- Strip photoresist and grow thick oxide in exposed areas.
- Remove Si₃N₄ using hot phosphoric acid.

## 2. Well Formation (Twin-Tub Process)

- Apply photoresist with Mask-2 to define N-well regions.
- Apply Mask-3 to define P-well regions.
- Perform ion implantation:
  - Boron for P-well.
  - Phosphorus for N-well.

## 3. Gate Terminal Formation

**Threshold Voltage Adjustment:**
- Mask-4: Implant Boron for PMOS.
- Mask-5: Implant Phosphorus or Arsenic for NMOS.

**Gate Formation:**
- Strip damaged oxide and re-grow gate-quality SiO₂.
- Deposit polysilicon.
- Pattern gate using Mask-6.

## 4. Lightly Doped Drain (LDD) Formation

- Mask-7: Lightly dope N⁻ for NMOS.
- Mask-8: Lightly dope P⁻ for PMOS.
- Deposit and etch oxide spacers using anisotropic etching.

**Purpose:** Reduces electric field near the drain to prevent hot carrier degradation in short-channel devices.

## 5. Source and Drain Formation

- Add screen oxide to prevent ion channeling.
- Mask-9: Heavily dope N⁺ for NMOS.
- Mask-10: Heavily dope P⁺ for PMOS.
- Perform high-temperature annealing to activate dopants.

## 6. Local Interconnect Formation

- Remove oxide over source, drain, and gate regions.
- Deposit Titanium to form TiSi₂ (silicide).
- Pattern using Mask-11.
- Clean residual Titanium Nitride using RCA cleaning.

## 7. Higher Metal Layer Formation

**Planarization:**
- Deposit doped oxide (PSG or BPSG).
- Perform CMP (Chemical Mechanical Polishing).

**Metallization:**
- Mask-12: Etch first contact vias.
- Mask-13: Deposit and pattern first Aluminum layer.
- Mask-14: Etch second contact vias.
- Mask-15: Deposit and pattern second Aluminum layer.
- Mask-16: Define final pad openings for bonding.

## Summary

The 16-mask CMOS process integrates NMOS and PMOS transistors using well-defined photolithographic and doping steps. This process enables high-performance, scalable, and reliable IC fabrication suitable for modern digital circuits.


# Lab:
## Cloning the VSD Standard Cell Design Repository in OpenLane

To begin working with the standard cell design lab in OpenLane, follow the steps below

Copy the clone URL from the repository and run the following command in the OpenLane terminal:

git clone https://github.com/nickson-jose/vsdstdcelldesign

- Then we have to open the mag file of the inverter which is being present as we are going to work on that inverted file only which is a .mag file and we are going to open it with the help of the magic tool and later calculate some of the metrics which is being done below:

![image alt]{https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(49).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(50).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(51).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(52).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(53).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(54).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(55).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(56).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(57).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(58).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(59).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(60).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(61).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(62).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(67).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(67).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(68).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(69).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(70).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(72).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(73).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(74).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(75).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(77).png)
- Rise Time : 2.23907e-9 - 2.17985e-9 = 0.05922ns
- Fall time : 8.09175e-9 - 8.05043e-9 = 0.04132ns
- Rise Delay : 2.20636e-9 - 2.14909e-9 = 0.05727ns
- Fall Delay : 4.07532e-9 - 4.05e-9 = 0.2532
# Introduction to Magic Tool Options and DRC Rules by Tim Edwards

This document introduces the use of the **Magic VLSI Layout Tool**, focusing on Design Rule Checking (DRC) and related tool options as explained by **Tim Edwards**.

## Reference Links

- **Skywater PDK Design Rules:**  
  [Skywater Design Rules Documentation](https://skywater-pdk.readthedocs.io/en/main/rules/periphery.html)

- **Official Skywater GitHub Repository:**  
  [github.com/google/skywater-pdk](https://github.com/google/skywater-pdk)

- **Magic VLSI Layout Tool (Tutorial & Docs):**  
  [opencircuitdesign.com/magic](https://opencircuitdesign.com/magic)

  - Recommended Sections:  
    - **Section 2:** Introduction to Layout  
    - **Section 6:** Design Rule Checking and Tool Configuration

## Lab Preparation

To begin working with the lab:

### Download Lab Files

Use the following command in your terminal:

- wget http://opencircuitdesign.com/open_pdks/archive/drc_tests.tgz
- tar -xvzf drc_tests.tgz

![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(78).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(79).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(80).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(81).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(82).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(83).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(84).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(85).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(86).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(87).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(88).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(89).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(90).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(91).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(92).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(93).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(94).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(95).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(96).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(97).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(98).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(99).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(100).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(101).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/de6d1479dc6cd23e641e8f22343abf32a2ae0e40/Day3/Screenshot%20(102).png)





