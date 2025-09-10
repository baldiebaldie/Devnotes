Computer Architecture
09-08-2025

### Levels of Program code
- Programs sit on top of many layers
- Application software is the apps you use (like word or chrome) which are written in high-level languages (like JavaScript and Python)
- System software helps apps run. This includes:
	- The compiler: Translator that changes high-level language to machine code
	- The Operating System: The manager that handles input/output, memory, storage, and shares CPU time between applications.
- Hardware: The actual machine parts (CPU, memory, Input/Output)
- You can think of program code like a restaurant. The customer (application) orders the food. The waiter (system software) takes the order and communicates it. The kitchen (hardware) cooks the food
### Instruction Set Architecture 
- An instruction set is the list of commands a computer understands
- Different computers have different sets, but many are similar. 
- Early computers had simple instruction sets to make building them easier
- Many modern computers also use simple sets (RISC(Reduced instruction set computer))
- More complicated computers use CISC (Complex instruction set computer)
### RISC-V Instruction Set

- RISC-V is an open-source instruction set.    
- Started at UC Berkeley, now managed by a foundation.    
- Used in many devices like cameras, printers, and network gear.  
- Think of RISC-V like a “public recipe book” that anyone can use and build from.

### Arithmetic Operations
- Basic operations like add subtract.
- Always uses 3 operands. Two inputs (a and b) and one result (c)
- You ideally want to keep things simple and regular. This makes computers faster and cheaper.
### Register Operands
- Arithmetic instructions mostly use registers
- RISC-V has 32 registers, each can hold 64 bits called a "doubleword"
- Different names for sizes
	- word (32b)
	- halfword (16b)
	- byte (8b)
	- nibble (4b)
	- and a crumb (2b)
- Instructions usually look like: `<operation> <destination>, <source1>, <source2>`
- Data goes into registers, is changed, then stored back.
- **Design principle**: smaller is faster. A smaller decoder is both cheaper and faster.
### RISC-V Registers (special roles)

- x0: always zero.
- x1: return address.
- x2: stack pointer.
- x10–x17: arguments for functions.
- x5–x7: temporary registers.  
    _Analogy: Each register is like a labeled drawer in a tool box with a special use._
### Simplified Names

- Tools like RARS let you use names like t0 (temporary), s0 (saved), a0 (argument).  
    Analogy: It’s like nicknames—easier to remember than long numbers.