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
