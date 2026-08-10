# RSIB Processor

## Overview

The RSIB Processor is an educational 8-bit RISC-style processor designed using Verilog HDL.

The project demonstrates the basic building blocks of a processor:

* Program Counter
* Program Memory
* Instruction Decoder
* Control Unit
* Register File
* Arithmetic Logic Unit
* Write-back logic

The design is verified using Verilog testbenches and RTL simulation.

## Objectives

* Design an 8-bit processor using Verilog HDL.
* Implement a register file.
* Implement an ALU.
* Implement instruction decoding.
* Implement a program counter.
* Execute arithmetic and logical instructions.
* Verify the processor using testbenches.
* Analyze RTL simulation waveforms.

## Specifications

| Parameter         | Value          |
| ----------------- | -------------- |
| Processor         | RSIB           |
| Architecture      | RISC-style     |
| Data width        | 8-bit          |
| Instruction width | 16-bit         |
| Registers         | 8              |
| Register width    | 8-bit          |
| Program Counter   | 8-bit          |
| ALU               | 8-bit          |
| HDL               | Verilog        |
| Simulation        | Icarus Verilog |
| Waveform Viewer   | GTKWave        |

## Architecture

```text
                   ┌──────────────────┐
                   │ Program Counter  │
                   └────────┬─────────┘
                            │
                            ▼
                   ┌──────────────────┐
                   │ Program Memory   │
                   └────────┬─────────┘
                            │
                            ▼
                   ┌──────────────────┐
                   │ Instruction      │
                   │ Decoder          │
                   └────────┬─────────┘
                            │
                 ┌──────────┴──────────┐
                 ▼                     ▼
          ┌──────────────┐      ┌──────────────┐
          │ Register     │─────►│     ALU      │
          │ File R0-R7   │      │              │
          └──────────────┘      └──────┬───────┘
                                       │
                                       ▼
                                  Write Back
```

## Instruction Format

```text
15       12 11       9 8        6 5        3 2       0
+----------+-----------+----------+----------+---------+
|  OPCODE  |    RD     |   RS1    |   RS2    | UNUSED  |
+----------+-----------+----------+----------+---------+
```

### Instruction fields

* `OPCODE`: Selects the operation.
* `RD`: Destination register.
* `RS1`: First source register.
* `RS2`: Second source register.
* `UNUSED`: Reserved bits.

## Instruction Set

| Opcode | Instruction | Description            |
| ------ | ----------- | ---------------------- |
| 0000   | ADD         | Add two registers      |
| 0001   | SUB         | Subtract two registers |
| 0010   | AND         | Bitwise AND            |
| 0011   | OR          | Bitwise OR             |
| 0100   | XOR         | Bitwise XOR            |
| 0101   | MOV         | Copy register          |
| 1111   | HALT        | Stop execution         |

## Processor Operation

The processor executes instructions using the following sequence:

```text
FETCH
  ↓
DECODE
  ↓
EXECUTE
  ↓
WRITE BACK
  ↓
FETCH NEXT INSTRUCTION
```

### Fetch

The Program Counter provides the address of the next instruction.

### Decode

The Control Unit extracts the opcode and register fields.

### Execute

The ALU performs the requested arithmetic or logical operation.

### Write Back

The ALU result is stored in the destination register.

## RTL Modules

### ALU

The ALU supports:

```text
ADD
SUB
AND
OR
XOR
MOV
```

### Register File

The register file contains eight 8-bit registers:

```text
R0
R1
R2
R3
R4
R5
R6
R7
```

### Control Unit

The Control Unit decodes the opcode and generates:

* ALU enable
* Register write enable
* HALT signal

### Program Memory

Program memory stores the instructions executed by the processor.

### RSIB Processor

`rsib_processor.v` is the top-level module connecting all processor components.

## Testbench

The testbench verifies:

* Reset
* Instruction fetching
* Program Counter
* ALU operations
* Register updates
* HALT instruction

Example test sequence:

```text
R1 = R0 + R0
R2 = R1 + R1
R3 = R2 - R1
R4 = R2 AND R3
R5 = R2 OR R3
R6 = R2 XOR R3
HALT
```

## Simulation

### Requirements

Install:

* Icarus Verilog
* GTKWave

### Compile

```bash
iverilog -o rsib_sim \
rtl/alu.v \
rtl/register_file.v \
rtl/control_unit.v \
rtl/program_memory.v \
rtl/rsib_processor.v \
testbench/rsib_processor_tb.v
```

### Run

```bash
vvp rsib_sim
```

### View waveform

```bash
gtkwave waveform.vcd
```

## Expected Simulation

```text
================================
      RSIB PROCESSOR TEST
================================

PC = 0
Instruction = 0200

PC = 1
Instruction = 0488

PC = 2
Instruction = 1650

PC = 3
Instruction = 28D8

PC = 4
Instruction = 3AD8

PC = 5
Instruction = 4CD8

HALT TEST : PASS

================================
SIMULATION COMPLETE
================================
```

## Test Cases

| Test  | Expected Result       |
| ----- | --------------------- |
| Reset | PC = 0                |
| ADD   | Correct addition      |
| SUB   | Correct subtraction   |
| AND   | Correct logical AND   |
| OR    | Correct logical OR    |
| XOR   | Correct logical XOR   |
| MOV   | Correct register copy |
| HALT  | Processor stops       |

## Applications

This project is useful for learning:

* Computer architecture
* Digital electronics
* RTL design
* Verilog HDL
* Processor design
* ALU design
* Register-file design
* Instruction decoding
* Hardware simulation

## Future Enhancements

* Add immediate instructions.
* Add load/store instructions.
* Add RAM.
* Add conditional branches.
* Add jump instructions.
* Add stack support.
* Add interrupts.
* Add UART.
* Increase data width to 16/32 bits.
* Implement pipelining.
* Implement on FPGA.

## Repository Structure

```text
RSIB-processor/
│
├── README.md
├── LICENSE
│
├── rtl/
│   ├── rsib_processor.v
│   ├── alu.v
│   ├── register_file.v
│   ├── control_unit.v
│   └── program_memory.v
│
├── testbench/
│   ├── rsib_processor_tb.v
│   ├── alu_tb.v
│   └── register_file_tb.v
│
├── simulation/
│   ├── run_simulation.sh
│   ├── simulation_results.md
│   └── waveform.vcd
│
└── docs/
    ├── architecture.md
    └── instruction_set.md
```

## Author

RSIB Processor developed as an educational RTL processor-design project using Verilog HDL.
