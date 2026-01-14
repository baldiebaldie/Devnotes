
Operating systems
08-25-2025


# What is an operating system
- An OS acts as an intermediary between the computer and the application 
- It is also software that converts hardware into applications
### What does OS provide?
- Abstraction: Provide standard library to access resources
- What is a resource?
	- Anything valuable(CPU, memory, disk)
- Examples of abstractions in an OS
	- CPU: processes and/or threads
	- Memory: Address space
	- Disk: Files
### Why should OS do this?
Advantages of OS providing abstractions:
- Allow applications to reuse common facilities
- Make different devices to look the same
- Provide Higher-level or more useful functionality
- OS controls access size to improve security (Protect application at a common layer)
- Provide efficient access to resources (cost, time, and energy)
- Provide fair access to resources
Challenges of OS
- What are the correct abstractions?
- How much of a hardware should be exposed (to improve security)?
- What are the correct mechanism?
- What are the policies?
What is sharing?
- Multiple users of the systems
- Multiple applications run by the same user
- Multiple devices for the same functionality
### Operating systems in three easy pieces
1. Virtualization
2. Concurrency
3. Persistance 
## Virtualization 
- The OS takes a physical resources and transforms it into a virtual form of itself.
	- This virtual form is usually more powerful and easier to use.
	- Sometimes, we refer to this OS as a virtual machine
	
  CPUs use time sharing
- >>**Think of this instead of each process is taking a piece of the CPU but each process has a small amount of time where it has the ENTIRE CPU. This concept is known as time sharing.**
- But with space sharing, applications are all given their own part of a computers resource at the same time opposed to getting the entirety of a computers resources for a small time. 
**When virtualizing the CPU**
 - the system takes the single CPU and makes a very large number of virtual cpus.
 - Then each unique process can use a "different" CPU allowing multiple processes to run at the same time
**When virtualizing memory**
- The physical memory is an array of bits
- A program keeps all of its data structures in memory
- read data (load)
- write data (store)
dynamic data in heap and static data in stack
### System calls
- OS offer interfaces for users (API)
	- allows to run programs
	- access memory
	- access devices
- Typically, an OS exports hundreds of system calls for applications
- System calls allow users to tell the OS what to do
### Resource manager
- The OS manages resources such as a CPU, memory, and disk.
- The OS allows
	- Many programs to run -> sharing the cpu
	- Many programs concurrently has access to their own instructions and data -> sharing memory
	- Many programs to access devices -> Sharing disks.
### Concurrency
- Concurrency in an OS refers to the ability to execute multiple processes or threads at the same time improving resource utilization and system efficiency.
- Problems arise when multiple processes access and manipulated shared resources at the same time leading to wrong outcomes/outputs
- For example, if two processes share the exact same data and try to run processes, the expected outcome can be skewed.
- This is a classic example of a **race condition**.
- Both threads are **racing to update** the same counter at the same time.
- Because they **don’t wait for each other**, some updates are lost.
- The computer switches between the threads **in the middle of an operation**, which breaks the counting.
![](../../../-images/Pasted%20image%2020250825151118.png)
![](../../../-images/Pasted%20image%2020250825151059.png)
### Persistance
- Devices such as DRAM store data in a volatile manner (can change at anytime)
- Hardware and software need memory to stay persistent 

