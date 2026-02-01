# Theory:
## Clock Gating, Delay Tables, Clock Tree Synthesis, and Timing Analysis

## 1. Clock Gating Technique

Clock gating is a power optimization method where the clock signal is selectively enabled based on control signals. This prevents unnecessary switching activity and reduces dynamic power consumption.

- **AND Gate Approach:**  
  Clock propagates only when the enable signal is `1`.

- **OR Gate Approach:**  
  Clock propagates only when the enable signal is `0`.

When the enable signal disables the clock, there is no short-circuit or dynamic power consumption. This principle is widely used in **clock tree design** to reduce switching activity.

---

## 2. Delay Matching and Skew in Clock Trees

In a clock tree, buffers are inserted at multiple levels to drive the clock signal to flip-flops across the chip. Each level of buffers can have different **load capacitances** and **sizes**.

If the load and buffer sizes at the same level are balanced, delay is consistent and **skew remains zero**.

### Skew Definition:
Skew = `t2 - t1`,  
Where `t1` and `t2` are clock arrival times at different flip-flops.

In large designs, even small skew can cause **timing violations**, making delay balancing crucial.

---

## 3. Delay Tables and Timing Models

Standard cells include timing models in `.lib` and `.lef` files.

Delays are mainly influenced by:

- **Input Slew:** Rate of voltage change at the cell input.
- **Output Load Capacitance:** Load seen at the output pin.

Delay tables map combinations of input slew and output capacitance to gate delay. These models are essential in tools like OpenSTA for accurate timing analysis.

---

## 4. Key Terminologies

| Term     | Definition                                                                 |
|----------|----------------------------------------------------------------------------|
| **Skew** | Difference in clock arrival times between flip-flops.                      |
| **Slew** | Rate of change of voltage on a signal over time.                           |
| **Latency** | Delay from clock source to endpoint flip-flop.                         |
| **Jitter** | Short-term fluctuations in clock edge timing due to PLL variations.      |

---

## 5. Setup Timing Analysis (Ideal Clock)

Assuming:
- **Clock Frequency** = 1 GHz
- **Clock Period (T)** = 1 ns

### Setup Equation:
```
Θ < T - S
```
Where:
- `Θ` = Total delay from launch flip-flop to capture flip-flop
- `S` = Setup time of the capture flip-flop

When considering **jitter**:
```
Θ < T - S - SU
```
- `SU` = Setup uncertainty due to clock jitter

Setup violations occur when the signal arrives too late at the capture flip-flop.

---

## 6. Clock Tree Synthesis (CTS) using H-Tree

To reduce skew, the clock distribution uses **symmetric routing structures** like H-Trees.

### Steps:
1. Calculate distances from the clock source to all endpoints.
2. Identify a central point to start routing.
3. Split the clock path symmetrically.
4. Insert buffers to maintain signal strength.

This ensures minimal skew by balancing the path length to each flip-flop.

---

## 7. Clock Buffers and Crosstalk

### Clock Buffers:
- Amplify and preserve clock signal integrity.
- Provide equal rise/fall times to reduce skew.

| Parameter       | Clock Buffers   | Data Buffers     |
|------------------|------------------|-------------------|
| Rise/Fall Time | Symmetrical     | Can vary          |
| Purpose         | Uniform timing  | Logic control     |

### Crosstalk:
Occurs when a signal from one wire (aggressor) interferes with another nearby wire (victim) due to **coupling capacitance**.

#### Effects:
- Glitches on victim nets
- Timing delays
- Signal integrity issues

#### Solution: Shielding
- Place grounded wires (VSS or VDD) between aggressor and victim nets.
- Prevents noise from coupling into sensitive clock nets.

---

## 8. Timing Analysis with Real Clocks

With real clock trees, buffers and routing introduce delay:

- `delta1`: Clock delay to launch flip-flop
- `delta2`: Clock delay to capture flip-flop

### Slack Calculation:
```
Slack = Data Required Time - Data Arrival Time
```
- Slack should be ≥ 0
- Negative slack indicates a timing violation

---

## 9. Hold Timing Analysis

Hold time ensures that the launched data remains stable long enough to be correctly captured.

### Key Points:
- Uses **same clock edge** for both launch and capture flip-flops.
- Violations occur when the data path is too **fast**.

### Influencing Factors:
- Low combinational delay
- Fast clock buffer delays
- Hold time of flip-flop

> Note: Clock period and jitter do not affect hold time analysis.

---

## Conclusion

In modern VLSI design, optimizing the clock network is essential for reliable and power-efficient performance. Techniques such as clock gating, balanced buffering, skew control through CTS, and accurate timing analysis using delay tables are key elements of any digital backend flow.

Maintaining proper timing margins ensures:
- Synchronous data transfer
- Minimum skew
- Reduced power
- Functional correctness of the entire chip

# Lab:

# Adding a New Standard Cell and Timing Analysis in OpenLane

## Steps to Convert Grid Information Into Track Information

- The `sky130_inv.mag` file contains detailed information about the standard cell such as:
  - Power and ground connections (P/G)
  - Port definitions
  - Logical function and internal layout

- OpenLane does not require full layout from `.mag`; it uses LEF (Library Exchange Format) which includes:
  - Cell boundary dimensions
  - Power and ground rail locations
  - I/O port locations

- Hence, `.mag` is converted to `.lef` for use in full-chip designs like `picorv32a`.

## Standard Cell Design Guidelines

- **Port Placement**: Place input/output ports at routing track intersections.
- **Cell Dimensions**: Height and width should be odd multiples of vertical/horizontal pitch.
- **Tracks**:
  - Defined by metal layers (e.g., met1, met2)
  - `tracks.info` file gives pitch, number of tracks, layer details

## LEF Extraction

- Ensure the layout aligns with the routing grid (tracks.info)
- Use `lef write` command in tkcon to generate LEF
- LEF files:
  - **Cell LEF**: Abstract view of cell (boundary, pins, layers)
  - **Tech LEF**: Routing layers, vias, DRC info

## Timing Libraries & Including New Cell in Synthesis

- Liberty files (.lib) define timing/power for cells under different PVT corners.
- Copy `sky130_vsdinv.lef` and `sky130.lib*` to `picorv32a/src`
- Modify `config.tcl`:
  - `LIB_SYNTH`, `EXTRA_LEFS`, etc.
- Run synthesis via OpenLane and verify if `sky130_vsdinv` is used.

## Running Synthesis with OpenSTA

```tcl
docker ./flow.tcl -interactive
package require openlane 0.9
prep -design picorv32a
set lefs [glob $::env(DESIGN_DIR)/src/*.lef]
add_lefs -src $lefs
set ::env(SYNTH_SIZING) 1
run_synthesis
```

- Create `pre_sta.conf` and `my_base.sdc` (clock, load, delay definitions)
- Run `sta pre_sta.conf`

## Synthesis ECO for Slack Fixing

- Edit OpenLane synthesis config using commands:

```tcl
prep -design picorv32a -tag 01-04_12-54 -overwrite
set ::env(SYNTH_STRATEGY) "AREA 0"
set ::env(SYNTH_SIZING) 1
run_synthesis
```

- If WNS and TNS are still negative, proceed to floorplan and placement:

```tcl
init_floorplan
place_io
tap_decap_or
run_placement
```

## Define Timing Constraints

- Edit `my_base.sdc` with:

```tcl
create_clock -period 10 [get_ports clk]
set_input_delay 2 -clock clk [all_inputs]
set_output_delay 2 -clock clk [all_outputs]
set_max_fanout 5 [all_outputs]
set_driving_cell -lib_cell INVX1 -pin Y [all_inputs]
set_load 0.5 [all_outputs]
```

- Re-run `sta pre_sta.conf`

## Netlist Replacement and Floorplan to CTS

- Replace old netlist with ECO netlist.
- Re-run:

```tcl
run_synthesis
init_floorplan
run_placement
run_cts
```

## Verifying CTS

- OpenROAD uses CTS script at `scripts/tcl_commands/cts.tcl`
- Configuration includes:
  - `CTS_CLK_BUFFER_LIST`
  - `CTS_ROOT_BUFFER`
  - `CTS_MAX_CAP`

## Real Clock Timing Analysis

```tcl
openroad
read_lef [path_to_merged.lef]
read_def [path_to_cts.def]
write_db pico_cts.db
exit

openroad
read_db pico_cts.db
read_verilog [netlist]
read_liberty [lib_files]
read_sdc my_base.sdc
set_propagated_clock [all_clocks]
report_timing
```

- Use only typical corners for real-clock-based STA.
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(103).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(104).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(105).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(106).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(106).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(107).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(111).png)
![image alt]{https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(112).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(113).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(114).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(115).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(116).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(117).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(118).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(119).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(120).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(121).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(122).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(123).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(124).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(125).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(126).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(127).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(128).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(129).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(130).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(131).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(133).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(134).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(135).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(136).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(137).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(138).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(139).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(140).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(141).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(142).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(143).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(144).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(145).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(147).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(148).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(149).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(150).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(151).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(152).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(153).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(154).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(155).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(156).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(157).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(158).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(159).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(160).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(161).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(162).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(163).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(164).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(165).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(166).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(167).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(168).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(169).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(170).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(171).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(172).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(173).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(174).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(175).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(176).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(177).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(178).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(179).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(180).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(181).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(182).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(183).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(185).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(186).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(187).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(188).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(189).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(190).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(191).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(192).png)
![image alt](https://github.com/ThivyaSekharPM/VSD-Digital-VLSI-SoC-design-and-planning/blob/c48eaded47dbfeaff8d0584d08f93297e3f5a48e/Day4/Screenshot%20(193).png)


