Operating systems
09-22-2025

#### OS in the early system
**Uni programming**
- Old computers ran only one program at a time
- The OS would load one program into memory and let it run until it was finished
- This was inefficient because the computer sat idle when the program was not actively using resources
- ---
### Multiprogramming and Time sharing
**Loading Multiple Programs**:
- Modern systems load several programs into memory at once
- The OS quickly switches between programs giving each one a turn to run. 
- This greatly improves efficiency because during the downtime of one program, say waiting to use I/O for example, another program can run in its place.
**The protection problem**
- When multiple programs share memory, there's a risk that one program could accidently or maybe intentionally access another programs memory 
- This could cause crashes or security issues
- ---
### Address Space: The core abstraction
**What is an address space**
- This is the OS's way of organizing all the memory that belongs to one program
- It contains everything a program needs: Code, data variables, etc. 
- Each program gets its own address space
1. Code segment:
	- Where the actual program instructions are stored
2. Heap: 
	- Used for dynamic memory allocation
	- When you use `malloc()` in C or 'new' in C++ memory comes from here
	- Grows as you allocate more memory during runtime
3. Stack:
	- Stores temp data like function call information, local variables, and return addresses
	- Grows when you call functions and shrinks when functions return
4. Free Space:
	- The gap between heap and stack
	- Both can grow into this space as needed
- ----
### Virtual vs. Physical Addresses 
**Virtual Addresses**
- Any address you see in your program is actually a virtual address, not the real location in physical memory
- when you print a memory address in C, you're seeing a virtual address
- every program thinks it starts at address 0 and has lots of memory.
**Physical Addresses**
- The actual location in the computers RAM chips
- programs don't see these directly
- The OS translates VA to PA behind the scenesa
---
### Goals of Memory Virtualization
1. Transparency
	- Programs shouldn't know that memory is being virtualized
	- Each program should feel like it has then entire computer to itself
2. Efficiency
	- Time efficiency: Translation shouldn't slow programs down too much 
	- Space efficiency: The structures used for virtualization shouldn't waste too much memory 
3. Protection
	- Programs must be isolated from each other
	- One program cant read or modify another programs memory 
	- The OS itself must also be protected from programs
- -- 
### How it all works together
The OS creates the illusion that each program has a large, private chunk of memory starting at address 0. In reality:
- Multiple programs share the same physical memory
- They're located at different physical addresses
- The OS and hardware work together to translate between virtual and physical addresses 
- Programs remain isolated and protected from each other
**Key Takeaway:** Memory virtualization solves the problem of safely running multiple programs at once. Each program gets its own "virtual" view of memory that seems private and starts at address 0, while the OS manages the actual physical memory behind the scenes.