# 8-Bit Pipelined Customized RISC Processor (32 Instructions)

A high-performance, modular Verilog implementation of an 8-bit RISC processor. This core features a **5-stage pipeline**, robust **hazard management**, and a specialized **32-instruction set** tailored for efficient embedded computation.

## 🏗 Architecture Overview

The processor is designed with a classic RISC philosophy, utilizing a 5-stage pipeline to achieve high throughput:

1. **Fetch (IF)**: Interfaces with the 256-byte Instruction Memory (`InstructionMemory.v`) and manages the Program Counter (PC). Supports multi-word instructions via a dedicated Instruction Register (IR).
2. **Decode (ID)**: Decodes the 32-instruction ISA. It manages a 4x8-bit Register File and houses the Condition Code Register (CCR) for status flag management (Z, N, C, V).
3. **Execute (EX)**: Performs arithmetic and logic via the ALU. Includes custom logic for stack pointer manipulation and branch target calculation.
4. **Memory (MEM)**: Interfaces with the 256-byte Data Memory (`Data_Mem.v`). Handles synchronous writes and asynchronous reads.
5. **Write-Back (WB)**: Routes computational or memory results back to the Register File or the Stack Pointer.

## ⚡ Key Technical Features

* **Custom 32-Instruction ISA**: Supports a wide range of operations including:
* **Arithmetic/Logic**: ADD, SUB, INC, DEC, etc.
* **Memory Access**: LDM (Immediate), LDD (Direct), STD (Direct), STI (Indirect).
* **Control Flow**: JMP, CALL, RET, RTI, and conditional jumps.
* **Stack Management**: Dedicated hardware support for PUSH and POP.


* **Advanced Hazard Unit**:
* **Data Forwarding**: Resolves Read-After-Write (RAW) hazards by bypassing data directly to the Execute stage.
* **Pipeline Stalling**: Automatically inserts bubbles for load-use hazards.
* **Control Flushing**: Clears the pipeline during jumps and returns to maintain execution integrity.


* **Interrupt Handling**: Integrated hardware support for external interrupts, including automatic CCR state saving and the `RTI` (Return from Interrupt) flow.

## 📂 Repository Structure

| File | Role |
| --- | --- |
| **`top.v`** | The top-level module interconnecting all stages and the Hazard Unit. |
| **`hazard_unit.v`** | Logic for stalling, flushing, and data bypassing. |
| **`ALU.v`** | 8-bit Arithmetic Logic Unit with status flag generation. |
| **`RegFile.v`** | 4-register file with specialized support for SP (R3). |
| **`*latch.v`** | Pipeline registers ensuring timing isolation between stages. |
| **`run.do`** | ModelSim automation script for compilation and waveform setup. |

## 🛠 Simulation & Testing

This project includes a complete testbench and automation script for ModelSim/QuestaSim.

1. **Prepare Software**: Load your machine code into `imem.hex`.
2. **Execute Script**: Run the following in the simulator console:
```tcl
do run.do

```


3. **Analyze**: The script automatically categorizes signals into "Global," "Pipeline," and "Register File" groups for easy debugging.

## 📋 Instruction Format

Instructions are 8 bits wide:

* **[7:4]**: Opcode.
* **[3:2]**: Source/Destination Register A.
* **[1:0]**: Source/Destination Register B.

*For multi-byte instructions (like LDM), the subsequent byte in memory is automatically captured as the immediate value.*
