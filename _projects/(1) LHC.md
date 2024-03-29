---
name: SmartL1-A Trigger Forwarding Algorithm for LHC
tools: [VHDL, Version Control, Testbed]
image: /cern.png
description: Implemented an algorithm to overcome data desynchronization between the on-detector front-end ASICs and off-detector FPGA readout systems in the Large Hadron Collider (LHC).
---

## Electronics in the Pixel Detector of LHC

The pixel detector is a huge array of CMOS sensors that capture the radiation emitted during the experiments. The pixels are interfaced to chips called front-ends that capture the data. The captured data is then sent over optical links to the processing system called Read-out Detector(RODs) which is a few 100 meters away isolated from radiation.

The front-end is an ASIC and ROD is an FPGA. The communicate using CERN's Slink protocol.

The ROD sends out a trigger signal whenever it wants the front-end to capture data. The front-end has a buffer that stores all incoming triggers when the no. of triggers are greater than it's processing time. When the buffer is full, the front-end skips triggers and sends a skipped trigger count to the ROD. 

---

## Problem

There is a bug in the ASIC which results in the front-end producing wrong skipped trigger counts. The ROD which is keeping track of the triggers it sent out and triggers it received gets confused as result. This results in a desynchronization. When the desynchronization occurs, all useful data is also discarded since it is not reliable.

---

## Solution

Keep track of the no. of triggers sent to the FE. If the number is greater than the size of the buffer, stop sending triggers. In-short, bypass the skipped trigger mechanism in the front-end and handle it in the ROD before it even goes out.

When a trigger needs to be skipped, an empty frame is created to keep the system in sync.

---

## Implementation

#### Step 1: Identify Triggers

The incoming control signal is sent serially. Therefore a shift register needs to be implemented to detect the pattern "11110" for a trigger.

#### Step 2: Keep track of the no. of triggers

For each new trigger, increment a counter. Decrement the counter whenever a module trailer is received from the front-end.

#### Step 3: Set the threshold

The threshold is configurable by the user, therefore implemented as a register. The user uses a command to write the threshold onto the register.

#### Step 4: Decide whether trigger is skipped or forwarded

If the value of the counter in step 2 is greater than the threshold in step 3, the trigger is not forwarded. Insert an empty frame for the module header.

#### Step 5: Update counters

Everytime a trigger is skipped, update a counter so we can monitor the inefficiency of the system.

---

## Slink Protocol

Header consists of a 32-bit word. There are bits to indicate L1ID, BCID, formatter number.

Trailer consists of a 32-bit word. Some bits in the trailer are set to indicate errors like timeout, dataoverflow etc.
