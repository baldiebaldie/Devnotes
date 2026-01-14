Computer Architecture
11-03-2025

### Processor Design Notes: Instruction-Level Parallelism

**What is instruction level parallelism (ILP)?**
- This is running multiple instructions at the same time to allow for quicker execution
2 ways to increase ILP
1. Deeper pipelines
	- Break each instruction into more steps
	- Each step does less work so it runs faster
2. Multiple Issue
	- Start multiple instructions in the same clock cycle
	- CPI becomes less than 1
	- We measure IPC (instructions per cycle) instead
i.e.
- A 4GHz processor with 4-way multipole issue can theoretically complete 16 billion instructions per second
- 4GHz 4,000,000,000 cycles per second
- 4 way so 4,000,000,000 * 4 = 16 billion

---
### Types of multiple Issue
Static multiple Issue (VLIW)
- The compiler groups instructions together before the program runs
- Instructions are packed into "issue slots"
- The compiler must avoid hazards
- VLIW = very long instruction word
	i.e: RISC-V dual issue packages:
	- One ALU/branch instruction + One load/store instruction per cycle
	- If you cant fill both slots, pad with "nop" (no operation)
Dynamic Multiple Issue (Superscalar)
- The CPU decides which instruction to run together while the program is running
- The CPU examines the instruction stream and picks safe combinations
- More flexible than static, but requires more complex hardware
Speculation: Guessing to go faster
- **Speculation** means the processor makes educated guesses about what will happen next, then fixes mistakes if the guess was wrong
Examples of speculation:
1. Branch speculation: Guess which way a branch will go and start executing those instructions
2. Load speculation: Guess what value will be loaded from memory. If guess is right, you save time. If wrong, you "roll back" and do it correctly
---
### Problems with Multiple Issue
New types of Hazards
- When instruction run out of order, new problems appear...
	- WAR (Write after read): An instruction tries to write a register that a later instruction is still reading
	- WAW (Write after write): Two instructions try to write to the same instruction
#### Solution: Register renaming
The processor secretly creates registers that the compiler doesn't know about. This eliminates false dependencies caused by reusing register names.

**Example**: Instead of both instructions writing to register f6, rename one to use a hidden register S.

---
### Loop Unrolling
Loop unrolling means copying the loop body multiple times to expose more parallelism.

Instead of doing 
```r
Loop: load, add, store, branch back


Do:
Loop: load1, load2, load3, load4, add1, add2, add3, add4, store1, store2, store3, store4, branch back
```
This gives processor more instructions to work with at once.

---
### Why dynamic Scheduling
Compilers cannot predict everything
- Cache misses happen unpredictably
- Branch outcomes change during execution
- Different processor models have different timing
Dynamic scheduling lets the hardware adapt in real-time
---
### Reality Check: Does multiple issue really work?
Yes, but not as much as we hope
There are some limitations:
- Programs have real dependencies that cannot be eliminated
- Memory is often slow compared to the processor
- It is hard to keep all parts of the processor busy
- The "instruction window" (how far ahead the processor can look ahead) is limited
**Bottom Line**: Multiple issues helps, but programs rarely achieve the theoretical maximum speedup due to real world constraints
---
### Key Takeaway
Modern processors use both pipelining and multiple issue to run programs faster, but the actual speedup depends heavily on the specific program and how well its instructions can run in parallel.

---
### Footnote WAW and WAR

WAR (write after Read)
- This happens when an instruction tries to write to a register before a previous instruction has finished reading the same register. 
WAW (write after write)
- This happens when two instructions both try and write to the same register. The issue happens because one might finish in the wrong order, leaving the wrong final value in the register.
This is all possible because modern processors try to execute instructions out of order to go faster but they must avoid WAR and WAW hazards to make sure programs still work correctly. They use special hardware to detect and prevent these problems.