Computer Architecture
09-08-2025

### Understanding performance: Instruction Count and CPI
- The instruction count is the number of instructions a program runs.
- This depends on:
	- the program itself
	- the ISA
	- and the compiler used
- You can think of this an instruction count like the number of steps in a recipe. the shorter recipe is usually faster to cook
--- 
### CPI (Cycles Per instruction)
- This is the average number of clock cycles each instruction takes.
- The CPI is affected by the hardware of the CPU (maybe the amount of cores)
- Some instructions take more cycles than others
- The average CPI depends on the mix of instructions
- If some tasks in your to-do list takes longer than others, the average time per task depends on how many easy vs hard tasks I have.
- CPI shows how efficient the CPU is for a mix of instructions
- Comparing different processors (with the same ISA) helps understand performance
## CPI Example – Choosing Between Two Code Sequences

Code Sequence 1:

- Instruction Classes: A=2, B=1, C=2
- Cycles: A=1, B=2, C=3
- Total instructions = 5
- Total cycles = 10
- Avg CPI = 10 / 5 = 2.0
    

Code Sequence 2:

- Instruction Classes: A=4, B=1, C=1    
- Total instructions = 6    
- Total cycles = 9    
- Avg CPI = 9 / 6 = 1.5    

 **Conclusion:** Even though Sequence 2 has more instructions, it runs faster because of a better CPI. The instructions it does run is overall easier
 
---
### Performance equation
- CPU time = Instruction count x CPI x Clock Cycle time
- CPU time = (Instruction Count x CPI) / Clock Rate
- Clock Cycle Time = 1 / Clock Rate

The key points are:
	A program is faster if it has
	- Fewer instructions
	- A lower CPI
	- Or a shorter clock cycle
### CPI Example: Comparing Two Computers

#### PC A:
- Cycle Time = 250ps
- CPI = 2.0
####  PC B:
- Cycle Time = 500ps
- CPI = 1.2
    
Which is faster?
- PC A: 250ps × 2 = 500ps per instruction
- PC B: 500ps × 1.2 = 600ps per instruction  

 **Conclusion: PC A is faster.**
- PC A is like a faster chef who takes fewer breaks. Even though each step takes longer (CPI), overall, the cooking is done faster.
--- 
### What affects performance?
- **Algorithm** – Changes the number and type of instructions.
- **Programming Language** – Some are more efficient than others 
- **Compiler** – Translates code in smarter ways.
- **ISA** – Defines the instructions and how efficient they can be.
The main performance equation:
	CPU Time = Instruction Count x CPI x Clock Cycle Time
	
You can imporve performance by:
- Reducing the number of instructions,  
- Lowering the average CPI,    
- Increasing clock speed (shorter cycle time).

### Example:  Compiler Impact

Compiler A:
- Instructions: 1.0B    
- CPU Time: 1.1s    

Compiler B:
- Instructions: 1.2B    
- CPU Time: 1.5s    

**CPI Calculation:**
- Clock time = 1ns = 1GHz clock rate    
- CPI_A = 1.1 × 1GHz / 1.0B = 1.1    
- CPI_B = 1.5 × 1GHz / 1.2B = 1.25    

**Conclusion:** Compiler A is more efficient (lower CPI).


### Clock Speed Comparison

If both programs finish in the same time, how fast does B’s clock need to be?

Use:  
**Clock Rate = (# Instructions × CPI) / CPU Time**

Result: Clock B needs to be 1.37x faster than Clock A.

**Analogy:** If someone walks slower, they need to take more steps per second to keep up.

---

### Power and Energy Use

### Power Formula:

**Power = Capacitance × Voltage² × Frequency**

### Power Trends:
- As clock rate increased, so did power.    
- Eventually hit a **"power wall"** – can't keep increasing speed due to heat limits.    

**Example:** Like a car engine that gets too hot if you go too fast for too long.

---

### Reducing Power Use
If we:
- Reduce load by 15%,    
- Lower voltage by 15%,    
- Lower frequency by 15%,    

Power drops to about **52%** of the original.

**Key Idea:** Small changes in voltage and frequency lead to big power savings.

### The Power Wall
- We cant lower voltage much more
- Chips leak energy even when idle
- Modern CPUs turn off unused parts (called dark silicon)

### Multiprocessors
- There is more than one processor core on a chip
- Programs must be written to take advantage of multiple cores. This concept is called parallel programming
- Requires managing:
	- Workload balance
	- Communication between threads\
- Its like having several chefs in a kitchen. Useful only if they work together efficiently.

### Amdahl's Law
- Improving one part of a system only helps as much as that part that is used
- The formula is:
	- Total =  Time for improved part / improvement + Time for rest of the system
- Examples:
	- If multiplication takes 80sec of a 100sec program,    
	- And you want 5× speedup overall...    
	- You’d need multiplication to run **instantly** (which is impossible).
- The takeaway is to focus on the parts used most often

### SPEC Benchmarks

- A way to compare CPU performance across systems.   
- Uses **geometric mean** to summarize results across many programs.
    

**Why geometric mean over arithmetic mean?**
- It treats all tests equally, no matter how long they take.    
- More fair than just averaging times.

If you’re comparing speeds in different races, don’t just average time — consider performance ratio.