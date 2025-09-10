Operating systems
09-08-2025

# Mechanism: Limited Direct Execution
### How to efficiently virtualize the CPU with control?
- The OS needs to share the physical CPU between processes using time sharing. Remember time sharing allows each process to use the entire CPU for a small amount of time creating the illusion of many CPUs.
- There are two main problems with this
	- Performance - how can me implement virtualization without adding excess overhead?
	  - Control - how can we run processes efficiency while retaining control over the CPU.
- To answer these questions requires help from both the hardware (CPU, memory, etc.) and the OS.
- Pretend the CPU is a shared classroom computer. Every student (process) wants to use it, but the teacher (OS) has control who uses it and when.
---
### Direct Execution (with no limits)
- Direct execution is the idea that the OS wants to run the program directly on the CPU because this is the fastest way to run a program
- This means the OS does not interfere with the program (it has full control)
- But if programs run freely its kind of bad
	- can lead to crashing the system or preventing other programs to run.
Its like giving some kids the keys to your car and letting them drive to school however they want. It is fast and perhaps convenient but very risky.
#### Problem 1: What if a process want to do a Restricted Operation?
- Programs should no be allowed to do certain things by themselves so they are restricted by the OS
- e.g.
	- talking to the hard drive
	- using up more memory
	- sending network data
#### The solution: It has to use  protected control transfer
- Two modes:
	 - User mode: where regular programs run
	 - Kernel mode: The OS runs here. It has full access to hardware
- Special instructions:
	- Trap: Switches from user model to kernel mode
	- Return from trap: Goes back from kernel mode to user mode.
- Trap table: 
	- Like a menu or map that tells the CPU which OS function to run when a program makes a request
	- Set up by the OS when the system starts
- Essentially, since the process is not allowed to do some things it can do a trap request to the OS who will go to process and then do a trap return to get back to the process
- We can think of the user mode as a **guest** at a hotel. Guests can request things, but only the **hotel staff (kernel mode)** can actually do them. The front desk (trap table) handles these requests and directs them to the right staff member (trap handler).
### System Calls
- A system call is how a program politely asks the OS to do something it cannot do directly.
- Examples are:
	- Reading/writing files    
	- Creating or ending processes
	- Talking to other processes	    
	- Requesting more memory 
The way it works is:
1. the program uses a trap instruction.
2. then the CPU switches to Kernel Mode
3. CPU uses the trap table to find the right OS function
4. OS runs the function
5. The OS uses return from trap to get back to the program

- Basically, when a trap is called, it is the process calling for tech support where you explain your issue (the call number), they look it up (trap table), fix it (handler), and then hang up (return to user mode).

### Security and User input
- The programs can make mistakes or try to cheat.
- The OS must check every request carefully.
- For example if a program tries to access and write to a bad memory address, if allowed it could
	- crash the system
	- access private OS data
	- Spy on other programs
	- etc.
- So to prevent this the OS must reject bad system calls
### Limited Direct Execution Protocol (review)
- The protocol lets programs run directly on the CPU, but with limits
- Programs run in user mode most of the time 
- If they need help or do something wrong, the OS steps in via:
	- system call (trap)
	- timer interrupt
- This way, programs run fast and the OS stays in control
- Note when kernel mode is popped and the OS takes over and then turned off, from the programs POV there is no interruptions
--- 
#### Problem 2: Switching between processes. 
- The OS MUST switch between programs to share the CPU. How can it take back control?
- **Cooperative Approach**
	- This is where the OS waits for a program to make a system call called yield( ) (this is where a program asks for a pause).
	-  This is also employed when the program throws an error.
	- But if the program never gives up, then it hogs the CPU forever
-  **NON Cooperative Approach**
	- OS does not wait:
	- it takes control using a timer interrupt
	- The way the timer works is:
		1. At boot time, the OS starts a timer
		2. after a few milliseconds, the timer is interrupted by the CPU
		3. then the OS takes over and decides whether or not to 
			- Let the current program keep going
			- or switch to a different program.
#### Saving and restoring Context
- When switching programs, the OS must remember where it left off
- It saves the state of the current program
	- stores in register values
	- program counter (Where it was)
	- and the stack pointer (points to which stack the current programs data is stored)
- After it saves, then it loads the saved state of the next program
#### Context Switch (putting it all together)
1. Timer interrupt happers
2. OS saves the current process to memory
3. OS loads the next process's state
4. CPU switches to that process and keeps running

- This is all done with low-level assembly code and it is very fast happening many many times per second.

- Note these registers are stored in a different stack all tied to a unique process. There is one stack for each process
#### What about Interrupts during interrupts?
- Sometimes, while the OS is handling one interrupt another one might happen
- The OS must protect itself from confusion or data corruption
- It does this by
	- Disabling interrupts while working on one
	- it uses locks to prevent data from being accessed by multiple processes at the same time
--- 
	### Key CPU Virtualization Terms

|Term|Simple Definition|
|---|---|
|**User Mode**|Safe, limited mode for programs|
|**Kernel Mode**|Powerful mode for the OS|
|**Trap**|Switch from user → kernel|
|**Return-from-trap**|Switch from kernel → user|
|**System Call**|A program's request for OS help|
|**Trap Table**|A map telling CPU which OS function to run|
|**Timer Interrupt**|OS tool to regain control of CPU|
|**Context Switch**|Save and load process state during switch|

---

## Summary

- Limited Direct Execution lets programs run fast, **but not too freely**.
    
- OS keeps control using **system calls** and **timer interrupts**.
    
- Context switching allows **many programs** to run on **one CPU**.
    
- The OS must always protect the system from **bad input**, **crashes**, or **bugs** in user programs.
    

>  **Final Analogy**: The OS is like a **class monitor**. Students (programs) can do their work, but the monitor watches the clock, manages the class, and keeps everything under control so no one goes wild.
