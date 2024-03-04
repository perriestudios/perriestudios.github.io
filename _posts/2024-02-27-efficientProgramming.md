---
title: Writing Efficient Programs
tags: [Programming]
style: fill
color: secondary
description: Write programs that not only work but are efficient
---

---
# First Principles

1. [Algorithm Efficiency](#algorithm-efficiency) - Clever techniques like iteration for fibonacci instead of recursion. Pay attention to time complexity. Use of efficient data structures.
2. Optimized Data Structures - Arrays are fast access but offer limited flexibility. Linked lists offer efficient insertion/deletion but slower in access.
3. Memory Management - Memory allocation and deallocation should be done cleverly to avoid **leaks**.
4. Loop optimization - Fewer loops means fewer branch instructions. Goal is to minimize no. of iterations use [loop unrolling](#loop-unrolling) or [loop fusion](#loop-fusion).
5. Reduce Function Calls - Minimize unnecessary function calls. Frequently used functions should be [inlined](#function-inlining).
6. Pointers - Use them for efficient memory access and manipulation.
7. Compiler Optimization - **Loop unrolling, function inlining, dead code elimination**
8. Cache Awareness - Memory in cache can be accessed faster. Memory access pattern can be optimized to maximize cache hits (but higher hits doesn't necessarily mean better, more costly memory access should be prioritized)
9. Avoid premature optimization - Write code first, profile it and then optimize the bottlenecks.
10. Efficient libraries - Standard library functions that are usually well optimized.
11. Testing and Profiling - Optimize the most critical performance bottlenecks.
12. Platform specific optimization - Use of platform specific APIs.

---

# Algorithm Efficiency

1. [Time Complexity](#time-complexity) - Amount of time a program takes to execute with scaling no. of inputs.
2. Space Complexity - Amount of space a program occupies in memory with scaling no. of inputs.
3. Best, Worst, and Average Case Scenarios - 
4. Optimality - Optimal algorithms achieve best possible time and space complexity. However, sometimes maintatining optimality becomes difficult due to other constraints.
5. Resource Utilization - Less CPU time, memory and network bandwidth.
6. Scalability - How well does it scale with growing no. of inputs.
7. Stability and Robustness - Should produce consistent results across different inputs and conditions.
8. Parallelism and Concurrency - Execute tasks in parallel and improve execution time.

---

## Time Complexity

Amount of work done by an algorithm as a function of the size of the input. 

To determine time complexity, all operations whose runtime is independent of input or output size/complexity.

Operations:
 - Integer addition
 - for(i=1;i<100;i++){A[i]=0;}

Not Operations:
 - for(i=1;i<\input_val;i++){A[i]=0;}   //Depends on input_val
 - count(list)				//Depends on list

## Time Complexity in Increasing Order

 - O(1) - Constant time complexity
 - O(logN) - Log time complexity
 - O(N) - Linear time complexity
 - O(NlogN) - Log Linear time complexity
 - O(N^2) - Quadratic time complexity
 - O(2^N) - Exponential time complexity 

## Array Time Complexity

 - Accessing an element - O(1) because the position is known
 - Inserting or deleting an element - O(n) 
 - Searching an element - O(n) because in worst case you have to look for all elements in array
 
---
 
## Function Inlining

A Function in the code are replaced with the contents of the function. This helps overcome the overhead of the function call such as pushing arguments into the stack, jumping to the function location and returning from the function.

However, it also increases the size of the program, thereby the instructions. It should be used when the performance benefits outweight the increased code size.

Compilers usually perform inlining, however, programmers can also explicitly specificy inlining through the **inline keyword** in C++. 

---

## Loop Unrolling

Execution of a loop - Updating loop variable, Checking loop condition, Execute Instructions, Branching.

With loop unrolling, the number of updates, checks and branching can be reduced, leading to better performance.

Loop unrolling can help improve Instruction Level Parallelism, memory access patterns(exploting spatial locality), more efficient pipelining(fewer branches equals fewer flushing)

---

## Loop Fusion

Take loop unrolling a bit further by merging two similar loops. 2x benefits eliminating the loop execution overhead. It can also improve cache performance if the two loops operate on the same data.

Process - Identify compatible loops, combine the bodies, eliminate redundant computations.

---
