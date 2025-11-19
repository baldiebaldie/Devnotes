Operating systems
11-10-2025


### Prototypical system architecture 
- Top level CPU and memory are connected through a memory bus (proprietary)
- Lower than that Graphics is connected through a general I/O bus (e.g. PCI)
- and then bottom level is the peripheral I/O bus connecting the disks and drives. 
A bus is a fancy way of saying these things are all connected through wires.
- Memory and graphics need to be fast which is why they are connected so close to the memory 
- Things like USB and SATA drives are further away from the processor. Those lines are thinner
![](../../images/Pasted%20image%2020251117121011.png)

### Modern architecture 
- CPU in the center and graphics and memory are connected on either side keeping close proximity to the processor connected by thick busses with many connections.
- The CPU is also connected to an I/O chip (a system in inside of itself)
- This I/O chip is itself connected to many things such as maybe the network, Disks, Keyboard/mouse, etc.
![](../../images/Pasted%20image%2020251117121044.png)

### Canonical Device
Has 2 areas
1. Interface
	- Has registers:
		- Status
		- Command
		- Data
	
2. Internals
	- Has components:
		- Micro-Controller (CPU)
		- Memory (DRAM or SRAM or both)
		- Other hardware-specific chips


![](../../images/Pasted%20image%2020251117121347.png)

The protocol the Interface follows is:
- First way for the device to be ready.
- Once ready write data to DATA register
- Then write command to COMMAND register
- Then wait until the device is done with your request

The issue with this is that it is inefficient. We do not want to poll

**The solution is to have interrupts.**
- Instead of doing nothing while switching from CPU to writing to DISK, you can now do an interrupt and allows another task to use the CPU while writing to the DISK
We can add an external CPU, a DMA controller, which soles purpose is to transferring data to the DATA register to optimize our interface protocol

![](../../images/Pasted%20image%2020251117122634.png)

### Implementations of a file system

![](../../images/Pasted%20image%2020251117123345.png)

- POSIX API - What defines function signatures like, open, read, write, close, etc.
- Generic Block Interface - This interface has even more primitive read and write.
	- Block is just bits that can be read and write
- Specific Block Interface