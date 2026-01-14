Operating systems
08-27-2025

### What is a process?
- the high level goal is to allow each process to ALONE use the CPU (This is called time sharing)
- Resources can be shared in time and/or space

> A process is a running program created and managed by the OS
- Programs are passive(stored on a hard disk) while processes are active
- The OS enables multiprogramming by virtulizing the CPU (gives each program some time with the FULL CPU). From the process CPU (there is only 1 CPU and every now and then it gets to use the full power of it)

Time sharing 
- allows the resource to be used for a little while by one entity
- e.g. CPU
- The cost of time sharing is potential performance
- note the resources are first come first serve 
Space sharing
- A resource is divided in space around those entities who wish to use it.
- e.g. Memory, Disk
Mechanism vs Policy
Mechanism
- Low-level methods or protocols which provides the answer to hows in a system
- e.g. How does an operating system perform a context switch?
Policy
- The decision making logic which provides the answers to whichs in a system
- e.g. which process should the OS run right now?

Contents of memory in its address space: Code, data, stack, heap, etc.
Contents of CPU registers: Data registers, Special registers (Program counter, stack pointer, frame pointer)

The OS has some key data structures that track various relevant pieces of information
	Process list
	- contains info about all processes in the system
	- ready processes
	- blocked process
	- current running process
	Register context: Contents of the CPU register

PCB - Process control block: Contains crucial information about a process. It acts almost like an ID card.  ![](../../../-images/Screenshot%202025-08-27%20at%202.21.31%20PM.png)

Process API:
An OS Application Programming Interface (API) is a set of functions and procedures that allow a program to interact with the OS to manage and control processes.
- It enables developers to create, kill, suspend, and monitor processes, as well as interact with their outputs and error streams
Typical OS process interfaces:  
- Create: Start a new process.  
- Destroy: Terminate a process.  
- Wait: Wait for a process to finish.  
- Miscellaneous Control: Suspend/resume processes.  
- Status: Get process state or resource usage.

---

Process Creation:
When you run a program, the operating system must first load it into memory. A program on your disk is just a file with a set of instructions. A **process** is the program when it's actively running.

- The computer's memory is divided into different areas for the process. These areas include the **code** (the program's instructions), **static data** (permanent information), the **heap** (for dynamic data), and the **stack** (for temporary data).

- The operating system doesn't load the entire program at once. It only loads the parts of the program that are needed right now. This is called **lazy loading**. It saves time and memory.

- This process transforms a static program file on the disk into an active process in the computer's memory, ready to be executed by the CPU.

---

Process States:
Processes can be in one of three different states

1. **running** when it's using the computer's CPU. The CPU is the brain of the computer. A running process is currently doing its work.

2. **ready** when it's prepared to run. However, the CPU is busy with another process. The ready process is waiting for its turn to use the CPU. It's like a student waiting in line to use a computer in a lab.

3. **blocked** when it's waiting for something to happen. For example, a process might need to read a file from the hard drive. While it waits for the data, it cannot use the CPU. This lets another ready process take its turn on the CPU.![](../../../-images/Screenshot%202025-08-27%20at%202.36.11%20PM.png)