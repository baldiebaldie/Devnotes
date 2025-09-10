Operating systems
09-08-2025

extra vocab:
	- image: A file containing all the necessary data, programs, and settings to deploy an OS to a computer. Acts as a template or a snapshot of a functional system.
	- mechanism: is the low level details of how an OS accomplishes something
	- Policy: The OS deciding what to do (e.g. which process to run next)
		- difference between policy and mechanism in an analogy is mechanism = car engine, brakes, and steering. Policy = the driver's decision on where to go.
	
--- 
### User mode vs Kernel mode:
- The OS separates user mode (This is where the program runs normally) from kernel mode (Where the OS has full control). This protects important hardware and sensitive parts in the system from accidently messing with hardware or sensitive operations.
- Think of the Kernel mode as the boss room in the company where only managers are allowed in and all of the important decisions are made. Outside of that room or "mode" the sensitive parts of the company are protected from being accidently altered.
---
### Processes [fork() and exec() system calls]
- **fork()** 
	- creates a new process from the parent process by copying the parent process. 
	- fork() returns 0 to the child process and the child's PID to the parent. 
	- After fork() the parent and child run independent processes (in sense a fork from the parent process).
	- imagine fork() is making a photocopy of a HW assignment - two identical copies exist.
- **exec()**
	- replaces a process's program with a new program
	- does not return if successful. When successful it replaces the process image.
	- Using exec() is like changing the "task" the process is doing without creating a new process
	- Going back to the homework analogy, exec() would instead be erasing the original copy of the homework by writing a completely new assignment on it.
---
### Address Space after fork()
- Parent and child processes have separate addresses spaces but use a technique called copy-on-write to save memory until they actually change something.
- Its like having two roommates who share a single textbook until one of them starts writing notes in it, then they have to get separate copies.
 --- 
### Process States
- The OS keeps track of whether a process is either running, ready, or blocked to manage CPU efficiency. 
- This helps the SO know what each process is doing and manage many processes.
- Similar to a track light system:
	- Green light = Running (processor is using the CPU)
	- Yellow light = Ready (Process is waiting to use the CPU to run)
	- Red light = Blocked (Process waiting for something else, like I/O to finish)
---
### Memory Management: Stack and Heap
- Stack:
	- stores function calls and local variables
- Heap
	- Used for dynamic memory allocation (like new objects or arrays during runtime)
- Both are used because some data is fixed in function calls and some data is more flexible and changes size throughout a process's lifetime.
---
### Interrupts, traps and Context switching
- Interrupt handler/ trap handled:
	- Part of the OS that deals with interrupts (signals to the CPU to stop and handle something)
	- Context switch means saving the current process's state (like registers) and loading another process's state, allowing multiple processes to share the CPU.
	- The reason registers are stored is because registers hold important info for the current process, and we don't want to lose any of that info when switching.
	- You can think of context switching like switching drivers in a car race. The pit crew saves the current driver's car's stats and prepares the next driver to take over smoothly
 --- 
### Process Control Block (PCB)
- PCB is a structure the OS uses to keep track of every process's state, memory, open files, and CPU registers.
 - You can think of this like a student's report card, keeping all of the information in one place.
---
### CPU Virtualization and Scheduling 
- The OS creates the illusion of multiple CPUs by rapidly switching between processes where each process has access to the entire CPU at one time.
- The scheduler decides which process gets the CPU next.
- Think of a juggler keeping many balls (Processes) in the air by quickly switching attention between all of them.
---
### wait() and waitpid()
- wait() lets a parent process wait for its child to finish and collect its exit status.
- waitpid() returns the PID of the child when finished.
- wait() is used for preventing zombie processes (a terminated child process that still has access to the process table), synchronizing process execution (allowing child process to finish first), and resource management.
---
### Time Sharing vs Space Sharing
- Time Sharing divides CPU time among processes by switching quickly
- Space sharing divides physical resources like memory among processes.
- In other words, time sharing is like taking turns on a single playground slide, while space sharing is like dividing the playground into sections for different kids.
---
### I/O Requests and Process States
- When a process requests I/O, it usually comes blocked until the I/O finishes. This lets the CPU run other processes in the meantime.

# Summary Table of Key Concepts and Analogies

| Topic                         | Concept                                  | Simple Analogy                                |
| ----------------------------- | ---------------------------------------- | --------------------------------------------- |
| User vs Kernel Mode           | OS protects hardware by modes            | Boss room vs office floor                     |
| fork() and exec()             | Create process / replace process image   | Photocopy homework / erase and write new work |
| Process States                | Running, Ready, Blocked                  | Traffic light system                          |
| Stack and Heap Memory         | Function calls vs dynamic memory         | Stack = book pile, Heap = storage room        |
| Address Space After fork()    | Copy-on-write memory                     | Sharing textbook until one writes notes       |
| Interrupts and Context Switch | Save/restore CPU state                   | Driver switching in a race                    |
| Mechanism vs Policy           | How vs what                              | Car engine vs driver decisions                |
| PCB                           | Process info storage                     | Student report card                           |
| CPU Virtualization            | Time sharing by fast switching           | Juggler keeping balls in air                  |
| wait()/waitpid()              | Parent waits for child                   | Parent waiting at school gate                 |
| Time Sharing vs Space Sharing | Divide CPU time vs divide physical space | Turns on slide vs playground sections         |
| I/O Requests                  | Process blocks during I/O                | Waiting for friend to fetch a book            |

### Practice problems
```C
#include <stdio.h>
#include <unistd.h>

int main() {
    fork();
    fork();
    printf("Hello\n");
    return 0;
}

```
![](../../images/Pasted%20image%2020250908115058.png)

Question 2:
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    int i;
    for(i = 0; i < 2; i++) {
        fork();
    }
    printf("Process running\n");
    return 0;
}

```
Since it loops twice, two forks are created
![](../../images/Pasted%20image%2020250908115058.png)