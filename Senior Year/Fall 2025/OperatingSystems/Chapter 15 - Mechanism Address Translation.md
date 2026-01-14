Operating systems
09-22-2025

### What is address translation?
- Address translation is like having a mail forwarding service. When a program thinks it's sending mail to "123 main street," the post office secretly forwards it to the real address "456 Oak avenue"
- All programs think they live at address 0, but actually live somewhere else in physical memory. Address translation converts the program's "virtual addresses" to the real "physical addresses" automatically.
- ---
### Why do we need address translation?
Operating systems have two main goals
- Speed
- Safety
Address translation helps achieve both through hardware support.
---
### How Base and Bounds work
This is the simplest translation method. The computer uses two special registers
1. Base register: Tells where the process actually lives in memory
2. Bounds register: Defines how much space the process is allowed to use

#### The translation formula
`Physical Address = Virtual Adress + Base Register`

#### Simple example
Image a process thinks it starts at address 0KB, but its actually loaded at 32KB in physical memory:
- Process wants to read address 128 -> Hardware translates to 32,896 (128 + 32KB)
- Process wants to read address 15KB -> Hardware translates to 47KB (15KB + 32KB)
#### Protection check
Before every memory access the hardware checks:
- Is the virtual address between - and the bounds limit?
	- If yes -> translate and access memory
	- If no -> trigger an error and likely terminate the process
`Virtual Address must be: 0 ≤ address < bounds`
	
**There are two ways to use the bounds register**
1. Store the size: Bounds = 16KB means "this process can use 16KB of space"
2. Store the end address: Bounds = 48KB means "this process ends at physical address 48KB"
Both methods work, its just different ways of marking the boundary
- -- 
### The Memory Management Unity (MMU)
- This is the hardware component that does the translations automatically on every memory access.
- Think of it as an automatic translator between what programs think they're accessing and where the data actually lives

**It performs 3 jobs**
1. Address translation: Adds base to every virtual address
2. Protection Control: Checks every address against bounds
3. Exception generation: Raise errors when processes misbehave
In user mode:
- The MMU does translation automatically. Programs run here
In Kernel mode:
- The OS runs here and can control the MMU directly.
**Key point**: This happens transparently - programs have no idea their addresses are being translated.
---
### Hardware requirements
For base and bounds to work, the hardware must provide:
1. **Privileged mode**: User programs can't modify base/bounds registers
2. **Base/bounds registers**: One pair per CPU
3. **Translation circuitry**: Hardware that adds and compares on every memory access
4. **Privileged instructions**: Special instructions to update base/bounds (OS only)
5. **Exception mechanism**: Ability to trap on illegal memory access
All modern processors include these features.


---
### Internal Fragmentation
This is wasted space inside a process's allocated memory
**Example**: A process gets 16KB of memory, but only uses 7KB. The unused 9KB (between the heap and stack) is wasted. The process keeps it allocated, but is sits empty.

---
### What the Operating System Must Do To Implement Base-and-Bounds

#### When a Process Starts
The OS must:
- Find free space in physical memory for the free list
- Allocate memory and remove that space from the free list
- Set the base and bounds registers
- Load the program into memory
- Jump to the program's starting instruction
Free List: A data structure tracking which parts of physical memory are available

#### When a Process Ends
The OS must:
- Stop the process
- Put its memory region back on the free list
- Remove the process from the process table
The freed memory can now be used by new processes.
#### During Context Switching (changing between processes)
The OS must:
- Save process A's register into the PCB
	- which includes process A's base and bound values
- Load process B's registers from its PCB
	- This includes process B's base and bound values
- Write B's base and bounds into the hardware registers
- Resume Process B

#### If Something Goes Wrong
When a process tries to access memory outside the bounds:
1. Hardware raises an exception
2. CPU switches to Kernel mode
3. OS exception handler runs
4. OS terminates the bad process
5. OS reclaims the process's memory
This prevents programs from crashing the whole system or stealing data from other programs

--- 
