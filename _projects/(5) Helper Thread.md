---
name: Lightweight Helper Thread for RISC-V Core
tools: [SystemVerilog, Verdi, CUDA Lite]
image: /helperThread.png
description: Designed a lightweight helper thread for the RISC-V Vanilla core used in the HammerBlade manycore. Implemented in a way similar to simultaneous multithreading, with the helper thread having a dedicated ALU while sharing the other resources in the pipeline. Benchmarked the performance of the system with memcpy.
---

## Motivation

- Vanilla Core has plenty of stall conditions
- With a background thread, the stalled time can be utilized to execute other instructions
- Similar to hyperthreading, a lightweight background thread called helper thread will run when the main thread is stalled.
- For this project, the helper thread will only execute integer instructions
- We started by identifying the conditions for core stalls

## Helper Thread Invoking

- Two fundamental stall signals in the RTL are stall_all and stall_id
- stall_all is used to stall all the stages of the pipeline
- stall_id is used to stall only instruction decode unit and associated components
- Helper thread will be invoked when stall_all is enabled since execute unit will not stall with stall_id

## Pipeline Resource Utilization

- 46x1024 icache virtually partitioned into 46x768 for main  and 46x256 helper threads
- Fetch unit is reused
- Separate PC, switching will depend on the stall signals
- Decode unit is reused
- Register File: 32 registers; 24 for main and 8 for helper thread virtually partitioned
- Execute Units: Separate ALUs
- LSUs, memory and write back will be reused

## Arbitration Logic for PC

- Vanilla Core should be initialized with two program counters
- PC1 will be initialized at 0 for the main thread
- PC2 will be initialized at 768 for the helper thread
- Switching between PC1 and PC2 will be based on the stall_all signal
- This logic was implemented inside the icache.v

## Arbitration Logic for Operand Storage

- Additional registers  need to be included to hold the state of the operands to be passed onto the execute units
- Since the stall scenarios are rather unpredictable, it is not wise to overwrite the operands of main thread with helper thread by reuse
- Register file to the flip flop path is switched using stall_all input

## Summary of RTL Changes

- Added PC2 and introduced arbitration between PCs
- Added arbitration for operand storage
- Added a helper ALU
- Added switching logic for the main and helper ALU results based on the stall_main signal
- Added arbitration logic for shared Decode, Memory Units
- Added new stalls pertinent to helper thread

## Assembly Program for Test

```
#include "bsg_manycore_arch.h"
#include "bsg_manycore_asm.h"
.text
// main thread code
_start:
   0:   02c02783                lw      a5,44(zero) # 2c <__bsg_id>
 4:   05802703                lw      a4,88(zero) # 58 <__cuda_barrier_cfg>
 8:   02112e23                sw      ra,60(sp)
 c:   00178093                addi    ra,a5,1
 10:   00209293                slli    t0,ra,0x2
    int sense = 1;
 14:   00100813                li      a6,1
 18:   01012623                sw      a6,12(sp)
    int cfg = __cuda_barrier_cfg[1+__bsg_id];
 1c:   00570333                add     t1,a4,t0
 20:   02812c23                sw      s0,56(sp)
 24:   02912a23                sw      s1,52(sp)
 28:   01812c23                sw      s8,24(sp)
    asm volatile ("csrrw x0, 0xfc1, %0" : : "r" (cfg));
 2c:   00032383                lw      t2,0(t1)
 30:   03212823                sw      s2,48(sp)
 34:   03312623                sw      s3,44(sp)
 38:   03412423                sw      s4,40(sp)
 3c:   03512223                sw      s5,36(sp)
 40:   03612023                sw      s6,32(sp)
 44:   01912a23                sw      s9,20(sp)
 48:   00050413                mv      s0,a0
 4c:   00058493                mv      s1,a1
 50:   00060c13                mv      s8,a2
  bsg_asm_finish(IO_X_INDEX, 0)
loop:
  beq x0, x0, loop
// helper thread code
.align 2 // 4-byte alignment
_helper_thread:

300:   000c8c93               addi      x25, x25, 0   
304:   0x001c8c93             addi      x25, x25, 1
308:   0x001c8c93             addi      x25, x25, 1
30c:   0x001c8c93             addi      x25, x25, 1
310:   0x001c8c93             addi      x25, x25, 1
314:   0x001c8c93             addi      x25, x25, 1
318:   0x001c8c93             addi      x25, x25, 1
31c:   0x001c8c93             addi      x25, x25, 1
```
