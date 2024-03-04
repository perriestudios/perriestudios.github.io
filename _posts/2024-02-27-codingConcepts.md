---
title: Coding Concepts
tags: [Programming]
style: fill
color: secondary
description: Review of important programming concepts for HW Engineers
---

# Pointers

Pointer stores the memory address of other variables. Efficiency of programs can be improved through memory access.

datatype * ptr;

ptr - name of the pointer
datatype - type of data the pointer is pointing to

Use of pointers involves: Declaration, Initialization, Dereferencing
```
int *ptr;		//Declaration - pointer of integer type is declared
int temp = 55;
ptr = &temp;		//Initialization - Address of variable is set to pointer
printf("Derefencing: %d", *ptr); //Dereferencing - Accessing value stored in memory through pointer
```

# Data Structures

Used to organize and hold a bunch of data used by the program.

## Array

## Stack

## Queue

## LinkedList

## Heap

## Binary Tree

## Graph

# Object Oriented Programming

# C++ Standard Template Library - Useful functions

There are 4 components: Algorithms, Containers, Functors, Iterators.

# C++ Keywords and Concepts

- Static - The variable get allocated space for the lifetime of the program. Subsequent calls to the variable will operate on the value stored in memory.

- Co-routine - Two functions exchanging controls within themselves without returning a value

# Memory Access

# Reference

[Geeksforgeeks](https://www.geeksforgeeks.org/data-structures/)
