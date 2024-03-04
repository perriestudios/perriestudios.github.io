---
title: Notes on ARM Architecture
tags: [Computer Architecture]
style: fill
color: primary
description: Focussed on A-profile. Most of the info is from ARM's documentation available online.
---

# ARM Variants

- A-profile (Applications): High Performance, Runs OS like Windows or Linux
- R-profile (Real-time): Targetted at systems with real-time requirements. Used in networking equipment, embedded control systems.
- M-profile (Microcontroller): Small, power efficient devices. For embedded and IoT applications.

# Architecture Overview

- Instruction Set: The function of each instruction. How that instruction is represented in memory (its encoding)
- Register Set: How many registers there are, the size of the registers, the function of the registers, the initial state of registers
- Exception Model: The different levels of privilege, the types of exceptions, what happens on taking or returning from an exception
- Memory Model: How memory acccess are ordered, how the caches behave, when and how software must perform explicit maintenance
- Debug, trace and profiling: How breakpoints are set and triggered, what information can be captured by trace tools and in what format

# Info about the Instruction Set

- Encoding: the representation of the instruction in memory.
- Arguments: inputs to the instruction.
- Pseudocode: what the instruction does, as expressed in Arm pseudocode language.
- Restrictions; when the instruction cannot be used, or the exceptions it can trigger.

# Execution Model

ARM follows a Simple Sequential Execution(SSE) model. The processor's behavior should be consistent with the behavior of one instruction executed at a time in the same order as they appear in the memory.

# Registers in ARM

## General Purpose
31 general purpose registers can be used as X or W:
- W0-W30: General purpose 32-bit registers
- X0-X30: General purpose 64-bit registers

Note: W0 will be bottom 32 bits of X0, W1 will be bottom 32 of X1 and so on. The choice of X or W will determine whether the instruction is executed as 32 or 64 bits. When a W register is written onto a X register, the top 32 bits are zeroed.

32 registers used for floating point or vector operations as Bx, Hx, Sx, Dx, Qx or V:

- B0-B31: 8 bits
- H0-H31: 16 bits (Half-Precision)
- S0-S31: 32 bits (Single Precision)
- D0-D31: 64 bits (Double Precision)
- Q0-Q31: 128 bits (Quad Precision)
- V0-V31: Vector register

## Other Registers
- XZR and WZR: Always 0 and ignore writes
- SP: Base address for loads and stores. There can be multiple stack pointers, each associated with an exception level. When SP is called, the one in the current execption level is used.
- LR(X30): Link register. Separate register ELR_ELx are used for returning from exceptions.
- PC: Program counter. General purpose register in some architectures, not in some.

## System Registers

System registers are used to configure the processor and to control systems such as the MMU and exception handling. System registers cannot be used directly by data processing or load/store instructions. Instead the contents need to read into an X register, operated on and then written back to the system register. There are two specialist instructions for accessing system registers:

- MRS reads system register into X
- MSR write X register into system register

System registers are specified by name. The names end with _ELx which specifies the minimum privilege necessary to access the register. EL1 requires EL1 or higher, EL2 requires EL2 or higher. Insufficient privilege will result in an exception.


# ARM Assembly

[ARM Assembly By Example](https://armasm.com/docs/fpu/overview/)

