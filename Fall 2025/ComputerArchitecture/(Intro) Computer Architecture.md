Computer Architecture
08-25-2025

### What is computer architecture?
- The design of the abstraction and implementation layers that allow is to execute information  processing application efficiently using manufacturing technologies
	- Essentially, if we abstract modern computing systems, computer architecture is in the middle between an application and physics(newton) in a device.
Deals with:
**Instruction set architecture (ISA)**
- Defines the interface between hardware and software (an instruction manual for what the processor should be able to do). 
- Example of this is RISC-V
**Microarchitecture**
The hardware that obeys the architectures abstraction
- uArch - implements the ISA
- Defines the particular implementation
- Defines the major structures, e.g. ALU, regfile, interconnects
**Register-Transfer level (RTL)** 
- Defines how the major structures function
- Defines how data moves between registers and how data are transformed in the process
- E.g. Behavioral Verilog  

The eight great ideas
1. Moores law
2. Abstraction to simplify design
3. Common case fast
4. Perfomance in Parallelism
5. Perfomance in Pipelining
6. Performance in Prediction
7. Hierarchy of memories
8. Dependability via redundancy 

Components of a computer
Same components for all kinds of computers
I/O includes
- User-interface devices
	- Mouse, keyboard, Monitor
- Storage devices
	- Harddrive, USB flashdrive, CD/DVD
- Network Adapters
	- For communicating with other computers'

--- 
Inside the Processor (CPU): 
Data path: performs operations on data
Control: sequences data path, memory
Cache memory:
- small fast SRAM memory for immediate access to data
- Many ways to organize the memory
- made possible with transistors

Performance Factors
Algorithm 
- determines number of operations executed
Programming language, complier, architecture 
- Determine number of machine instructions executed per operation 
Processor and memory system
- Determine how fast instructions are executed
I/O system (including OS)
- Determines how fast I/O operations are executed

2 ways to talk about performance
1. Response time
	- How long it takes for a processor to execute an instruction
2. Throughput
	- The amount of tasks completed per unit of time
A way to measure Performance could be 1/execution time

"X" is n times faster than "Y"
Performance_x / Performance_y

So if PC_a takes 15s to do a task and PC_b takes 10s then 

15/10 = 1.5 or PC_a is 50% Faster than PC_b

Elapsed time is the total response time including all cycles.
- This determines system performance
CPU time
- Time spent processing a given job
- Note, different programs are affected differently by CPU and system performance

CPU Clocking
Frequency and Cycle time

CPU Performance can be calculated by:

CPU Clock cycles / Clock rate