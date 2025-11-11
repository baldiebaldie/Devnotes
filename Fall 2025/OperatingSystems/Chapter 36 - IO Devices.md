Operating systems
11-10-2025

### Prototypical system architecture
- High performance devices are stored closer to the CPU
- Low performance devices are stored further from the CPU

### Modern system architecture
- CPU connects to I/O chip via a number of different interconnections

### Canonical device
Canonical devices have two important components
- Hardware interface: Allows the system to control its operation
	- Registers: (status, command, data)
- Internals: Implemented specifically
	- micro-controller (CPU)
	- Memory (DRAM or SRAM)
	- Other hardware-specific chips
Implementing the abstraction the device presents to the system


How does the CPU communicate with IO devices?
By reading and writing three registers, the OS can control device behavior.
- Status register: current status of the device
- Command register: tell the device to perform a certain task
- Data register: Pass data to the device, or get data from the device
