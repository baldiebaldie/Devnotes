Computer Architecture
10-22-2025
## Pipeline Hazards

**What are hazards?** Hazards happen when instructions in a pipeline can't work properly together. Think of it like cars on an assembly line - sometimes one car needs something that the previous car hasn't finished yet.

**Types of hazards:**

- **Data hazards**: One instruction needs data from another instruction that hasn't finished yet
- **Load-use hazards**: A special type where one instruction loads data from memory, and the next instruction tries to use that data right away
	
**Example from the slides:**

```
lw t1, 12(t1)    // Load data into t1
add t5, t1, t7   // Try to use t1 right away - this causes a problem!
```

The add instruction can't get the value of t1 because the load instruction hasn't finished putting the data there yet.

**Solutions:**

- **Forwarding**: Send data directly from one stage to another without waiting
- **Stalling**: Make the processor wait until the data is ready

## Branch Prediction

**What is branch prediction?** Processors try to guess which way a branch (like an if statement) will go before they know for sure. This helps keep the pipeline moving fast.

**Why do we need it?** When the processor hits a branch instruction, it doesn't know whether to fetch the next instruction or jump somewhere else. Guessing wrong means wasting time.

**Types of predictors:**

- **1-bit predictor**: Remembers what happened last time
- **2-bit predictor**: Needs to be wrong twice before changing its guess

**Example:**

```
Loop: addi t0, t0, -1
      bne t0, zero, Loop
```

In a loop like this, the branch is "taken" (jumps back) most of the time, but "not taken" on the last iteration.

- 1-bit predictor gets 50% accuracy (wrong at start and end of loop)
- 2-bit predictor gets 75% accuracy (only wrong at the end)

## Exceptions and Interrupts

**What are they?** Unexpected events that force the processor to stop what it's doing and handle something else.

**Types:**

- **Exceptions**: Problems inside the processor (like trying to use an instruction that doesn't exist)
- **Interrupts**: Signals from outside devices (like a keyboard or mouse)

**How the processor handles them:**

1. Save where it was in the program
2. Save information about what went wrong
3. Jump to special code that fixes the problem
4. Either continue the program or stop it

**Examples of exceptions:**

- Trying to divide by zero
- Using an illegal instruction
- Hardware malfunction

**Why it's tricky in pipelines:** Multiple instructions are running at once, so multiple problems can happen at the same time. The processor needs to figure out which problem to handle first and clean up the mess properly.

The slides show that handling these situations gets very complex in modern processors, but the basic ideas help keep computers running smoothly even when things go wrong.
