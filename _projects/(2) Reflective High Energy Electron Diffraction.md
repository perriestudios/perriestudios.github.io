---
name: FPGA-accelerated machine learning for Reflection High-Energy Electron Diffraction(RHEED)
tools: [High Level Synthesis for Machine Learning, Python, Vivado HLS, Ultrascale+]
image: /fastML.jpg
description: Deploying a CNN on a FPGA for Reflective High Energy Electron Diffraction(RHEED). The FPGA will be interfaced with a camera to obtain inputs, and perform inference to predict the expected structure of the material.
---

## Problem

Currently the available RHEED systems can only analyse the structure of a crystal once it’s static due to long processing time. They do not offer real-time monitoring of the crystal structure during the growth phase. Therefore making it impossible to know whether the crystal being formed is as expected during the growth phase.

---

## Solution

Use a Convolutional Neural Network to analyze incoming images from a camera and analyze the position of crystal growth based on a Gaussian function. The neural network is based on LeNet5.

It consists of two layers performing 2D Convolution, 2D Batch Normalization, ReLU and 2D Max Pooling followed by FC, ReLU, FC1, ReLU, FC2 to derive the result.

---

## HLS4ML

This project converts a given python ML model into a C++ version that can be processed by a HLS tool. [HLS4ML Project](https://fastmachinelearning.org/hls4ml/index.html).

Each layer can be optimized for Resource or Latency in the model. You can have a combination of Resource and Latency too by editing model.

---

## Optimizing deployment

![HLS4ML Pipeline](/hls4mlOptimization.png)

#### Python Model

- Identify bottlenecks in the code and reduce them. This could be done by merging loops together, etc.

#### HLS4ML

- Resource or Latency
- Reuse Factor
- Precision of data type (ap_fixed<16,6> is arbitrary precision fixed point 16,6)

#### Manual optimization of Generated C++

Go over the C++ code and optimize anything you feel can be optimized

#### Vivado HLS

Use pragmas:

- HLS Latency
- HLS Performance
- HLS Pipeline
- HLS Unroll

#### RTL

Manually pipeline stages if you need to. This is by far the most complicated.

#### Vivado

- Constrain the clock for a lower period and do synthesis.
- Use directives for Synthesis, Optimization, Floorplanning, Placement and Routing.
