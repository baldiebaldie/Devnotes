Computer Architecture
11-10-2025

### Part 1: fundamental processor architecture 

Key conceptual understanding
Every instructions goes through the same steps
1. IF (instruction fetch): Fetch the instruction from memory
2. ID (instruction decode): Decode what the specific instruction means
3. EX (execute): Begin to execute the instruction
4. MEM (memory): Access the memory if needed
5. WB (Write back): Write back the results if needed
The Processor's Datapath is designed around supporting these operations for ALL instruction types.

#### Why do we need different components for different instruction types?
- R-type instructions NEED ALU + register file to cook
- Load/store instructions NEED ALU (to calculate the resulting address) + data memory + sign extension
- Branch instructions need ALU (for comparison in the conditional) + calculate the target where it needs to branch to

#### Well then why can't we just connect everything directly?
- We need multiplexers to route the right data at the right time
- Different instructions use the same hardware components (Like the ALU) differently


#### Datapath design intuition
Think of the processor like a factory assembly line
- Instruction memory = the recipe book
- Register file = workspace storage
- ALU = the main processing unit
- Data memory = external storage warehouse
- the control unit = the foreman directing operations
I need to understand that every wire and multiplexer exist for a reason, to support the instruction execution model

---
### Part 2: Control and ALU design
Conceptual framework:
- The control unit is basically a lookup table that says "For THIS instruction type, generate THESE control signals"

ALU Control philosophy:
Why do we have two level control (Main control -> ALU OP -> ALU Control)?
1. For modularity: Easier to design and debug
2. Efficiency: ALU Control can be optimized independently
3. Flexibility: Easy to add new operations

Key insight:
- Main control decides what kind of operation family
	- Arithmetic, memory, branch
- ALU control decides which specific operation
	- Add, Subtract, AND, OR, etc.

Design patterns to recognize:
1. Load/Store always uses ADD (base + offset)
2. Branches always use subtract (for comparison (if reg1 - reg2 = 0 then both hold the same value and are equal))
3. R-type uses the instructions function field (most complex case. Note the function code determines which operation needs to be used like add, sub, and, or, etc.)

NOTE: If I understand WHY each instruction type needs specific ALU operations, you can derive the control signals rather than memorizing them.

---

### Part 3: Pipelining - "The game changer *Kappa*"
The fundamental insight of pipelining:
Pipelining is like an assembly line - instead of completing one instruction fully before starting the next, we overlap their execution stages.

Why pipelining works
- Different stages use different hardware (no resources conflicts known as structural hazards)
- Instructions are independent (When there are no hazards)
- Throughput increases without changing individual instruction latency

Performance intuition:
- Hazards create pipeline stalls
- Imbalanced pipeline stages limit clock speed (The longest instruction dictate the clock speed for the entire processor. Usually load/store instructions because they have to go to memory)
- Overhead from pipeline registers and control

Key insight: Pipelining improves throughput ( the amount of instructions completed per second ) but not latency (the time for a single instruction)

---

### Part 4: Hazards - the enemies of pipelining (<----aura)
Conceptual framework for hazards
There are 3 main types of hazards
1. Structural hazards = Resource conflicts
	- The issue happens when at least two instructions want to use the same hardware at the same time
	- The solution is to add more hardware or tank the stall
	- Note this hazard can be completely circumvented through design. Having a good ISA/Processor design can completely eliminate the event of two instructions needing the same hardware at the same time.
2. Data hazards = Information dependencies
	- This hazard happens when one instruction needs the result from a previous instruction
	- The way to fix this is through forwarding (bypass the pipeline registers with data) or tanking a stall
	- An example is an add instruction (add x1, x2, x3) directly followed by (sub x4, x5, x1). The issue is the sub instruction uses x1 and thus the sub instruction has to wait for the add instruction to fully compute before its able to do anything with that register.
3. Control Hazards = Direction dependencies
	- These issues arise when we have no idea which instruction to fetch next until the branch condition is resolved.
	- In other words, the branch instruction must be first evaluated before the ISA can know what instruction to run next
	- The way to deal with this is using predictions, delayed branches, or just tanking the stall
Design philosophy for solving hazards: We want to keep the pipeline full for as much as possible

#### Forwarding logic:
- When can we forward?
	- When the data is ready earlier than the writeback to the register file (usually in the EX stage)
- Where do we forward from?
	- Pipeline register outputs (once again usually in the EX stage)
- Where do we forward to?
	- ALU inputs (most common)
Why can't forwarding solve everything?
- Load - Use hazards: This is when we have a load instruction directly followed with a computation that uses the register that is actively being loaded
	- Memory data is not ready until the MEM stage
	- You cannot forward back in time
#### Intuitive Hazard recognition:
Quick Mental check:
1. Does instruction B need a result from instruction A? -> Potential data hazard
2. Is instruction A a load and instruction B uses the loaded value immediately? ->Definite  Load-use Hazard
3. Is instruction A a branch? -> Potential control hazard