# RTL-to-GDSII-Implementation-and-PPA-Optimization-of-a-Configurable-32bit-ALU
# RTL-to-GDSII Implementation and PPA Optimization of a Configurable 32-bit ALU

## Project Overview

This project focuses on the RTL design, logic synthesis, timing analysis, and physical-design exploration of a configurable 32-bit Arithmetic Logic Unit (ALU) using Verilog HDL and Cadence EDA tools.

The project aims to understand the complete digital IC implementation flow, from RTL description and functional verification to synthesis, PPA analysis, and physical-design exploration.

**Project Title:** RTL-to-GDSII Implementation and PPA Optimization of a Configurable 32-bit ALU Using Cadence Genus and Innovus

## Objectives

- Design a configurable 32-bit ALU using Verilog HDL.
- Implement arithmetic, logical, shift, and comparison operations.
- Perform RTL functional verification using a Verilog testbench.
- Synthesize the design using Cadence Genus.
- Apply timing constraints using Synopsys Design Constraints (SDC).
- Analyze area, timing, and power reports.
- Explore physical-design implementation using Cadence Innovus.
- Understand clock-tree specifications and physical-design concepts.

## Tools and Technologies

| Tool / Technology | Purpose                                   |
| ----------------- | ----------------------------------------- |
| Verilog HDL       | RTL design                                |
| Cadence Genus     | Logic synthesis                           |
| Cadence Innovus   | Physical-design exploration               |
| SDC               | Timing constraints                        |
| Verilog Testbench | Functional verification                   |
| Linux Terminal    | Script execution and project management   |
| Git and GitHub    | Version control and project documentation |

## ALU Architecture

The ALU accepts two 32-bit operands, `A` and `B`, along with a 4-bit opcode.

### Supported Operations

| Opcode | Operation | Description                    |
| ------ | --------- | ------------------------------ |
| `0000` | ADD       | Adds A and B                   |
| `0001` | SUB       | Subtracts B from A             |
| `0010` | AND       | Bitwise AND                    |
| `0011` | OR        | Bitwise OR                     |
| `0100` | XOR       | Bitwise XOR                    |
| `0101` | NOT A     | Bitwise complement of A        |
| `0110` | SLL       | Logical left shift             |
| `0111` | SRL       | Logical right shift            |
| `1000` | SLT       | Signed less-than comparison    |
| `1001` | PASS A    | Passes operand A to the output |

### Inputs

- `A[31:0]`: First operand
- `B[31:0]`: Second operand
- `opcode[3:0]`: Operation selector

### Outputs

- `result[31:0]`: ALU result
- `zero`: Indicates whether the result is zero
- `carry_flag`: Carry or no-borrow status, depending on the operation
- `overflow_flag`: Signed arithmetic overflow status

The exact behavior of the status flags depends on the implemented RTL and must be verified against the testbench.

## Design Flow

```text
        Verilog RTL Design
                |
                v
       Functional Verification
                |
                v
       Timing Constraints (SDC)
                |
                v
       Cadence Genus Synthesis
                |
                v
       Synthesized Gate-Level Netlist
                |
                v
       Area, Timing and Power Reports
                |
                v
       Cadence Innovus
                |
                v
       Floorplanning and Placement
                |
                v
       Clock-Tree Synthesis (if required)
                |
                v
       Routing and Physical Verification
                |
                v
       Final Layout / GDSII Export
```

*Note: The flow diagram represents the intended implementation workflow. The completed stages depend on the actual design and tool runs. Clock-tree synthesis is applicable when the design contains real clocked elements and requires a physical clock network.*

## Project Directory Structure

```text
ALU-32bit-RTL-to-GDSII/
│
├── rtl/
│   ├── alu_top.v
│   ├── alu_logic.v
│   ├── adder_ripple.v
│   ├── adder_cla.v
│   └── adder_brent_kung.v
│
├── tb/
│   └── tb_alu.v
│
├── constraints/
│   └── constraints.sdc
│
├── scripts/
│   └── script.tcl
│
├── reports/
│   ├── area.rpt
│   ├── timing.rpt
│   └── power.rpt
│
├── netlist/
│   └── alu_netlist.v
│
└── README.md
```

This is the proposed repository structure. Include only files that are actually implemented and available in your repository.

## Timing Constraints

The initial SDC configuration uses a virtual clock with a period of 2 ns.

| Constraint           |   Value |
| -------------------- | ------: |
| Virtual clock period |  2.0 ns |
| Input delay          |  0.2 ns |
| Output delay         |  0.2 ns |
| Input transition     | 0.05 ns |
| Output load          |    0.02 |

The input and output port names in the SDC must match the RTL module declarations. The clock period is an initial analysis target and does not itself establish that the design meets timing.

## Cadence Genus Synthesis

The synthesis script performs the following operations:

1. Loads the target standard-cell library.
2. Reads the Verilog RTL source.
3. Elaborates the top-level ALU module.
4. Loads timing constraints.
5. Checks the design.
6. Performs synthesis and optimization.
7. Generates area, timing, and power reports.
8. Exports the synthesized Verilog netlist.

### Running the Synthesis Script

From the project directory, launch Cadence Genus and execute:

```tcl
source script.tcl
```

The exact synthesis commands may depend on the installed Genus version and the configured technology library.

### Generated Reports

- `area.rpt`: Reports the synthesized cell area.
- `timing.rpt`: Reports timing paths and timing analysis results.
- `power.rpt`: Reports estimated power consumption.
- `alu_netlist.v`: Contains the synthesized gate-level netlist.

Power estimates depend on the activity assumptions, timing constraints, and technology library used during analysis.

## PPA Optimization

PPA stands for:

- **Power:** Estimated power consumption of the circuit.
- **Performance:** Timing delay and timing-constraint compliance.
- **Area:** Total standard-cell area used by the synthesized design.

The optimization process involves examining synthesis reports, identifying critical paths, evaluating logic structure, and comparing results under consistent constraints and library settings.

If multiple adder architectures are implemented, they should be synthesized and compared independently under the same conditions.

## Physical Design Using Cadence Innovus

The next stage explores the physical-design flow, including:

- Design import and technology setup
- Floorplanning
- Power planning
- Standard-cell placement
- Clock-tree specification and clock-tree synthesis, where applicable
- Routing
- Timing and physical verification
- GDSII export, where supported by the available flow

The Clock Tree Debugger interface is used to explore clock-tree information when the relevant specification and design data are available. Opening the debugger or creating a default clock-tree specification does not mean that clock-tree synthesis has been completed.

## Verification and Validation

Functional verification should cover:

- All supported ALU opcodes
- Addition and subtraction boundary cases
- Logical operations
- Shift operations
- Signed comparison cases
- Zero-result detection
- Carry and overflow behavior
- Reset or clock behavior, if present in the actual design

The RTL should pass simulation before synthesis results are treated as validated.

## Results

Record actual measured results from the Cadence reports.

| Metric                  | Result                            |
| ----------------------- | --------------------------------- |
| Cell area               | To be populated from `area.rpt`   |
| Critical-path delay     | To be populated from `timing.rpt` |
| Power estimate          | To be populated from `power.rpt`  |
| Functional verification | To be updated after simulation    |
| Physical-design status  | In progress                       |

Only verified results should be entered in this table. Comparisons between different implementations should use the same constraints, technology library, and analysis conditions.

## Learning Outcomes

This project provides practical exposure to:

- RTL coding and digital logic design
- Functional verification
- Logic synthesis using Cadence Genus
- Timing constraints and static timing analysis
- Area, power, and performance evaluation
- Physical-design concepts using Cadence Innovus
- Debugging synthesis scripts and tool errors
- Version control and technical documentation

## Acknowledgment

I would like to express my sincere gratitude to **Dr. R. Umamaheswari Ma’am** for her valuable guidance, encouragement, and support in strengthening my knowledge of VLSI design and industry-standard EDA tools.

Her guidance motivates me to continue learning and improving my practical skills in digital design and physical design.

## Author

**Durga Devi A**

VLSI Design | RTL Design | ASIC Synthesis | Physical Design

## Disclaimer

This repository documents an educational and engineering project. Technology libraries, synthesis reports, netlists, and physical-design outputs may depend on institutional tool access and licensing. Proprietary foundry libraries and licensed EDA files should not be uploaded publicly.

## Keywords

VLSI, Verilog HDL, 32-bit ALU, RTL-to-GDSII, Cadence Genus, Cadence Innovus, ASIC Design, Logic Synthesis, Static Timing Analysis, PPA Optimization, Physical Design, Semiconductor Engineering

<img width="1362" height="716" alt="ALU_32_90nm" src="https://github.com/user-attachments/assets/b0a096ed-b9d0-47d7-bbd6-e4781e9a75cf" />
