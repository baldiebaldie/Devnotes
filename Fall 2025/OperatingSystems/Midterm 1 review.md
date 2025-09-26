Operating systems
09-24-2025

## Chapter 2: intro to operating systems
#### What is an OS?
- A manager of your computer with two jobs
	1. Abstractions: When the OS makes hard things easy for programs to use. Instead of each program having to know how to talk to the drive and CPU the OS simply provides simple commands that works the same way for all programs.
	2. Resources management: The OS shares the computers resources fairly between different programs. This makes sure one program doesn't have all of the memory or the CPU
#### Three main concepts in OS
1. Virtualization - making one physical thing (memory, CPU, etc.) into many virtual things
2. Concurrency - running multiple things at the same time
3. persistence - keeping data safe even when the computer is turned off
---
## Chapter 4: Processes
#### What is a process?
- A process is a running program. When you click on a program icon, the OS creates a process for it
- A program sitting on your hard drive is just code. A process is the code actually running with its own memory space and resources.
#### Process components
- Running: currently using the CPU
- Ready: Waiting for the CPU to become available
- Blocked: Waiting for something like file reading to finish
#### Machine state
- This is everything the OS needs to remember about a process when its not running. This includes all of the CPU registers, the program counter, and memory information.
#### Mechanism vs Policy:
- Mechanism: HOW something is done
- Policy: WHICH choice had to be made
---
## Chapter 5: Process API
#### Key system calls
- Fork() - creates a copy of the current process. After fork runs, you have two identical processes. One child and one parent. The parent returns the PID of the child process and the child process returns 0.
- Exec() - Replaces the current process with a new program. The old program is gone and the new one takes its place.
- wait() - Makes a parent process wait until its child process finishes
- Why separate fork() and exec()?
	- This separation lets the parents set up things for the child (like redirecting I/O) before the child becomes a different program
---
## Chapter 6: Limited Direct Execution
**The problem:** How can the OS let programs run fast on the CPU while still maintaining control?
**The solution**: Let programs run directly on the hardware for speed, but limit what they can do.
#### Two main CPU modes
1. **User mode:** Programs run here with restrictions
2. **Kernel mode:** OS runs here with full access

**System calls:** when a program needs to do something that is restricted (like reading a file), it makes a system call. This uses a special "trap" instruction that switches to kernel mode and jumps to OS code.
**Timer Interrupts:** The OS sets a time that interrupts whatever is running ever few milliseconds which prevents any program for hogging the CPU
**Context switching:** This is how the OS saves the state of one process and loads the state of another. 

#### **CPU scheduling basics**
**Turnaround time**: How long from when a job arrives to when it finishes
**Response time:** How long from when job arrives until it first gets scheduled
Waiting time: How long a job spends waiting in queues

#### **Scheduling algorithms:**
**First come First served:** Run jobs in order that they arrive. Simple but can cause long waits if the first job is a long one.
Note: The convoy effect is where long-running processes block shorter, faster processes from accessing resources.

**Shortest Job First**: Always run the shortest job next. Good for turnaround time but requires knowing job lengths

**Shortest Time to Completion First**: Like SJF but can interrupt running jobs if a shorter one arrives
**Round Robin**: Most fair. Each job gets a small time slice, then moves to the next job. Bad for turnaround time

#### **Multi-Level Feedback Queue**
- the goal: Schedule jobs well without knowing how long they will run
How it works:
- Multiple priority queues
- New boss start at the highest priority
- Jobs that use their full time slice gets demoted
- Jobs that give up the CPU early (uses I/O and is in general slower) stay in high priority
Key rules:
1. Higher priority jobs run first
2. Same priority jobs run in RR
3. New jobs start at top priority
4. Jobs that use up their allowed time allowance get demoted
5. periodically boost all jobs back to top priority (prevents starvation)
Sometimes the process tries to slyly game the system but doing some trick to stay high priority.
Gaming Prevention: Better accounting tasks tracks total CPU time used, not just individual bursts.

#### **Fair-Share scheduling**
**Lottery scheduling**: Give each process "tickets". Randomly pick a ticket to decide which process runs next. More tickets = higher chance at being picked.

Stride scheduling: More deterministic version of lottery scheduling where each process has a "stride" (large number divided by its tickets). Then track each processes "Pass value". Always run the process with the lowest pass value. 

Linux CFS: Linux has a "Completely Fair Scheduler" which tracks "Virtual runtime" for each process. Then it will always run the process with the lowest virtual runtime. It also uses "nice" values to give some processes priority

#### Key concepts or success
Process creation steps:
1. Load program form disk to memory
2. set up stack and heap
3. set up I/O
4. Jump to main() function

System call processes: 
1. Program makes system call
2. Hardware trap instruction switches to kernel mode
3. OS handles the request
4. Return-from-trap switches back to user mode

Context switch steps
1. Timer interrupt stops current process
2. save the current processes state
3. OS scheduler picks next process
4. Load next processes state
5. Jump to next process
----
Remember:
	The key to mastering this material is understanding that the OS is constantly juggling between giving programs what they need while maintaining control and fairness. Every design decision involves tradeoffs between performance, fairness, and complexity.
	
---
## Additional cliff notes
#### What is Machine state?
Think of machine state like its a snapshot of everything happening in the computer at one given moment in time. Its all the information needed to know exactly what a running program is doing. 
#### Components of Machine state
**CPU Registers**: These are like the computers scratch paper. The CPU uses registers to do quick calculations and hold temp values. These are the different types:
- Regular data registers that hold number
- Special registers like the program counter
- Stack pointer (where we are in the programs memory stack)
**Memory Contents**: This includes the data the program is using
- The programs code itself
- Variables the program created
- the programs stack(for function calls)
- The programs heap (for dynamic memory)
I/O information: What file the program has open, what devices its using and similar resources
#### Why save machine state during context switch
The context switch process
1. Save the current process: The OS creates a snapshot of everything
2. Load new process: The OS takes process B's previously saved snapshot
3. Resume execution

####  Key Takeaway 
Machine state is like a complete "save game" file for a running program. The OS must carefully save this information during context switches so programs can resume exactly where they left off. 
Without this mechanism, multitasking would be impossible, and only one program could run at a time on your computer. This is why context switching is both essential for multitasking and a source of performance overhead - the OS is constantly playing an elaborate shell game with program states to create the illusion that multiple programs are running simultaneously.

---
## What is address space?
Address space is like giving each program its own private neighborhood in the computers memory. Every piece of data in a program has a unique memory address in memory

#### The house analogy
Think of computer memory like a giant apartment building with millions of rooms. Each room has a number (address) Without an operating system, programs would have to know the exact room numbers where their data lives. This creates issues:
- Two programs might try and use the same room
- One program might accidentally mess with another programs stuff
- Programs would break if moved to different rooms
The OS solves this by giving each program its own virtual apartment building. The program thinks it owns rooms 0,1,2,3, etc. But the OS secretly translates these to the real room numbers in the actual building.

#### Components of Address space
Every programs address space has four main sections:
- **Code Section (Text)**: This contains the programs instructions
- **Data sections**: This holds global variables and static data that the program declared. Think of it as storage closets that exist for the entire life of the program.
- **Stack**: When a function gets called, a new "Tray" or "Plate" gets added to the top of the functions local variables. When the function finishes, its tray gets removed. The stack grows and shrinks automatically
- **Heap**: This is free-form storage space. When a program asks for memory dynamically, it gets space from the heap. The program must manually give this space back when its done with it.
#### Why address space matters
**Protection**: Each program thinks it owns all the memory from address 0 to some maximum. Program A cannot accidentally write to Program B's memory because they have separate address spaces.
**Flexibility**: Programs can be written as if they're the only thing running. They don't need to worry about where they'll be placed in physical memory
**Memory management**: The OS can move programs around in physical memory without the programs knowing which is efficient.
#### What is a Process Control Block (PCB)
**Process Identification**:
- Process ID number (Kinda like a social security number for processes)
- Parent process ID (Which process created this one)
- User who owns this process
CPU state information:
- All the CPU register values when the process was last running
- Program counter (Which instruction to run next)
- Stack pointer and other special registers
Memory management information:
- Where the process's address space is located in physical memory
- Page tables that translate virtual addresses to physical addresses
- Memory limits and permissions
I/O information:
- List of files the process has open
- Network connections the process is using
- Other devices the process can access
Scheduling Information:
- Process priority level
- How much CPU time the process has used
- when the process was last scheduled.

#### Why PCB is essential
Context switching: When the OS needs to switch from Process A to Process B, it uses the
PCBs:
1. Save Process A's current state to its PCB
2. Load Process B's state from its PCB
3. Process B continue exactly where it left off
Resource management: The PCB helps the OS track what resources each process is using. When a process ends, the OS can clean up all its files, memory, and other resources.
Security: The PCB contains permissions that control what each process can access. This prevents programs from interfering with each other
## Key Takeaways

**Address Space:** Each process gets its own private view of memory, making programming simpler and systems more secure.
**PCB:** The OS's detailed record about each process, containing everything needed to pause, resume, and manage that process.
Together, these concepts allow multiple programs to run simultaneously on one computer while staying isolated from each other and sharing resources fairly.