### Hazards in pipelining
- Structural hazards: Same piece of hardware is in use when another wants to use it
- Data hazard: When you have to wait for an instruction to finish before you can continue
- Control hazard: Occurs when a pipeline cannot immediately fetch the next instruction because a conditional branch instruction's outcome. Also its target is not known yet

#### Forwarding/bypassing
One solution to a data hazard to wait. If you pipeline immediately, your value might be wrong by executing too quickly
Forwarding - Bypassing
- Definition: Technique that routes the result of an instruction directly from the output of one pipeline stage to the input of an earlier stage avoiding a stall that would be caused
- Don't wait for it to be stored in a register
- Requires extra connections in the Datapath
- This basically completely avoids stalling 

#### Load-Use data hazard
A load-use data hazard is a specific type of data hazard where an instruction tries to immediately use the result of a value that has just been loaded from memory by a preceding load instruction, but the value has not yet reached the required pipeline stage

Example:
```r
ld x1, 0(x2)

sub x4, x1, x5
```

Since the sub instruction contains x1, we cannot just run it right after the load instruction because x1 is not even loaded in yet. 

Think of it like a cook (instruction) trying to chop a vegetable (use data) the instant it leaves the fridge (load instruction), before it even has been placed on the cutting board (available in the register)

#### Multi-Cycle pipeline Diagram
A visual representation that tracks the progress of multiple instructions across various clock cycles and pipeline stages simultaneously showing how different instructions overlap in execution.
- MCP diagram is useful for visualizing hazards
- Shows which instruction is executing in which stage
- Hazards are dependencies backward in time
Example of an MCP:
![](../../images/Pasted%20image%2020251020161548.png)

An easier way to write it is in blocks kind of like this:

![](../../Pasted%20image%2020251020161856.png)

Best way is to go down by columns rather than full rows.
How to count the amount of cycles for given assembly code?
- Take 5 steps for the first instruction
- Add 1 for each subsequent instruction 
- Add 1 for each stall in the series
![](../../Pasted%20image%2020251020165259.png)


#### Pipeline benefits/misconceptions 
- Pipelining does not reduce latency but instead might even increase it. The execution / clock frequency might increase.
- Pipelines increase throughput
	- More instructions per unit of time
- Max speedup depends on number of stages
- ideally all stages take the same time
- Max speedup almost is never achieved
	- Pipelines stalls (hazards)
	- Time to "fill" and "drain" the pipeline reduce benefit
	- Longest stage determines clock (critical path)
- Usually consider steady state speedup i.e. speedup assuming pipeline is full
- Steady state - `Ignore drain and build up time and focus on speedups `

### Stall on branch (Control hazard) and Branch Prediction
- Wait until branch outcome is determined before fetching the next function.
- Longer pipelines can't readably determine outcomes early
	- Stall penalty becomes unpredictable
- Predict outcome of branch
	- Only stall if prediction is wrong
- Best case scenario is 0 delay
- Worst case scenario is 1 delay
	- This is much better because instead of having guaranteed 1 delay, you have a chance at getting 0 delay
- In RISC-V Pipeline
	- can predict branches not taken
	- Fetch instruction after branch with no delay
- The guess the hardware takes is mostly based off past instructions earlier in the lifecycle

### Pipeline big picture / Takeaway
- Pipelining improves performance by increasing instruction throughput
- It does this by:
	- Executing multiple instructions in parallel
	- Having each instruction have the same latency
- It does have issues:
	- Hazards (structural, data, and control)
	- Instruction sets design affects complexity of pipeline implementation