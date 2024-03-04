---
title: An index page of computer architectures
tags: [Computer Architecture]
style: fill
color: primary
description: Collection of various computer architectures.
---
# INDEX

- CPU Architectures
    - [In-order](#in-order-processors) 
    - Out-of-order
    - Multi-core
    - Many-core
    - VLIW 
    - SIMD: Vector and Array
 
- GPU Architectures
    - Edge Computing
    - Exascale

- Domain Specific Accelerators
    - Tensor Processing Unit
    - Vision Processing Unit
    - AI

---

# CPU 

CPUs are optimized for executing a set of instructions as fast as possible. They are heavily optimized for speed. Uses sophisticated hardware for control.

## In-order Processors

***Instructions get executed sequentially one after the other. Categorized under control flow and SISD.***

Advantages:
 - Simplicity in design, verification, and debugging.
 - Power efficiency due to simpler control logic and lower resource consumption.
 - Area efficiency with fewer transistors and less silicon area required.
 - Predictable performance characteristics, advantageous in real-time or safety-critical applications.
 - Potentially higher clock speeds due to simpler design and reduced complexity.

Disadvantages:  
 - Does not exploit any parallelism (instruction, thread, data, or task) - Less effective utilization of resources such as execution units and memory bandwidth. Usually execution unit is idle when the it's a memory load/store instruction.
 - Lower instruction throughput - Max. Instructions Per Cycle (IPC) can be 1 when pipelined.
 - Subject to stalls during memory fetches - Limited performance in complex workloads with high dependencies or irregular memory access patterns. 

## Out-of-Order Processors

***Instructions get executed in a dataflow order. Exploits instructions level parallelism to reduce processor stalls.***

Advantages:
 - More utilization of resources - Both execute and memory units can operate on seperate instructions.
 - Higher throughput

Disadvantages:  
 - Increased design, verification, and debugging.
 - Higher power consumption
 - Larger area
 - Limited scalability - Benefits of instruction-level parallelism (ILP) may diminish with more cores.

# References

- A nicely tabulated Wikipedia [Reference](https://en.wikipedia.org/wiki/Scalar_processor#References)
- Onur Mutlu's Lectures
