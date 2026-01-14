Operating systems
10-20-2025

### What are threads
Threads are like having multiple workers in the same office. Each worker has their own registers and know what their working on via the program counter. They all share the exact same bit of memory 

**Key differences from processes:**
- Threads share the same memory space
- Processes have separate memory spaces
- Context switching between threads is faster because you don't need to change the memory map
### Thread memory layout 
- In a single threaded program you have one stack at the bottom of memory
- With multiple threads each thread gets its own stack placed throughout the memory
- This means that each thread has its own local variables, but they can all access the same global data.

### Simple example
The chapter shows a program which creates 2 threads. One prints A one prints B.
The key insight is you cannot predict the order of expectation. Might be 
- A then B
or
- B then A
Both can even run at the same time on different processors
This unpredictability is what makes concurrency in programming challenging

### The big problem: Shared Data
There is an example in the book where two threads run at the same time and use the same global data, as threads do. Each thread adds 1 to a total 10 million times each. You would expect the output to be 20million but instead it came out as 19,345,221 and if ran again, it would be a different number

**Why this happens**: The simple operation `counter = counter + 1` actually takes 3 assembly instructions
1. load
2. add 1 
3. store

**A race condition happens when the timing of thread execution affects the final result**

Example: **Thread 1**: 
- Loads counter (value = 50) into register 
- **Thread 1**: Adds 1 (register now = 51) 
- **INTERRUPT**: Thread 1 gets paused, Thread 2 starts running 
- **Thread 2**: Loads counter (still 50) into its register 
- **Thread 2**: Adds 1 (register = 51) 
- **Thread 2**: Stores 51 back to counter 
- **INTERRUPT**: Thread 2 gets paused, Thread 1 resumes **Thread 1**: Stores 51 back to counter
Result: Counter = 51 instead of 52. One increment was lost!

---

### Key terms to know
Critical section
- Code that accesses shared data and shouldn't be run by multiple threads at the same time
Race condition
- When the final result depends on the random timing of thread execution
Indeterminate
- A program whose output changes from run to run based off race conditions
Mutual Exclusion
- Making sure only one thread can be in a critical section at a time

---

### The solution Atomicity
- We need operations to be atomic, meaning they happen "all or nothing" with no interruptions if possible
- Since we cannot have custom atomic instructions for every possible operations we need general building blocks that help us create atomic sections of code

--- 
### Another type of problem: Waiting
- Sometimes one thread needs to wait for another thread to finish before it can continue.
- i.e 
	- If thread A saves data to disk, thread B might need to wait until that's done before it can read the data
- This is different from the shared data problem but equally important in concurrent program.
---
### The big picture
- Concurrency makes programs much more complex because you lose the simple, predictable flow of single-threaded programs. 
- Instead of instructions happening in a clear order, you have multiple streams of execution that can interleave in unpredictable ways.
- The goal is to use hardware features and OS support to build tools that let programmers write concurrent code that works correctly despite this complexity. Without proper synchronization, concurrent programs produce random, incorrect results.



