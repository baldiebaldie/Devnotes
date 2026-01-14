Computer Architecture
09-08-2025

### **Levels of Program code**
- Programs sit on top of many layers
- Application software is the apps you use (like word or chrome) which are written in high-level languages (like JavaScript and Python)
- System software helps apps run. This includes:
	- The compiler: Translator that changes high-level language to machine code
	- The Operating System: The manager that handles input/output, memory, storage, and shares CPU time between applications.
- Hardware: The actual machine parts (CPU, memory, Input/Output)
- You can think of program code like a restaurant. The customer (application) orders the food. The waiter (system software) takes the order and communicates it. The kitchen (hardware) cooks the food
- ---
### **Instruction Set Architecture** 
- An instruction set is the list of commands a computer understands
- Different computers have different sets, but many are similar. 
- Early computers had simple instruction sets to make building them easier
- Many modern computers also use simple sets (RISC(Reduced instruction set computer))
- More complicated computers use CISC (Complex instruction set computer)
---
### **RISC-V Instruction Set**

- RISC-V is an open-source instruction set.    
- Started at UC Berkeley, now managed by a foundation.    
- Used in many devices like cameras, printers, and network gear.  
- Think of RISC-V like a “public recipe book” that anyone can use and build from.
---
### **Arithmetic Operations**
- Basic operations like add subtract.
- Always uses 3 operands. Two inputs (a and b) and one result (c)
***Design Principle 1: Simplicity favors regularity***
- You ideally want to keep things simple and regular. This makes computers faster and cheaper.
- ---
### **Register Operands**
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
***Design principle 2*** : smaller is faster. A smaller decoder is both cheaper and faster.
---
### **RISC-V Registers (special roles)**

- x0: always zero.
- x1: return address.
- x2: stack pointer.
- x10–x17: arguments for functions.
- x5–x7: temporary registers.  
    _Analogy: Each register is like a labeled drawer in a tool box with a special use._
---
### **Simplified Names**

- Tools like RARS let you use names like t0 (temporary), s0 (saved), a0 (argument).  
    Analogy: It’s like nicknames—easier to remember than long numbers.
---
### **Memory Operands**
- Memory stores bigger data such as arrays and structures.
- Registers hold the smallest amount of memory but are extremely fast
- The process is:
	1. Load values from memory into registers
	2. Perform arithmetic on registers
	3. Store results back into memory if needed
- Memory is byte addressed which means each address points to 1 byte.
- RISC-V uses little endian. This means that the lowest byte is stored at the lowest address. It store the "least significant byte first"
----
### **Memory Operand Example**

- C code: `A[12] = h + A[8];`    
- h is in x21, base address of A is in x22.    
- Compiled RISC-V code:
    
```scss
    // load A[8] (8*8 bytes = 64) 
    ld x9, 64(x22)   
    // add h + A[8]  
    add x9, x21, x9
    // store into A[12] (8*12 = 96)
    sd x9, 96(x22)   
```

If you want to access the code in the memory address A[8] you have to go 8 blocks of bytes over to access that area in memory. So 8 * 8 = 64. So since A is in x22 to load that data into register x9 you do 64(x22). Same thing to get to A[12]. 8 * 12 = 96. so 96(x22) accesses that memory address

---
### **Registers vs Memory**
- Registers are much faster than memory
- Memory use requires extra load/store instructions.
- Compilers try to keep variables in registers, and only use memory for less important data.
- This concept is called register optimization
- You can think of it like it is faster to use your head than to look at notes, but you only have limited brain space (registers)
---
### **Immediate Operands**
- An immediate operand is a constant written directly into the ISA
- things like `addi x22, x22, 4` adds 4 to the register x22
- This avoids needing to load 4 from memory
_**Design Principle 3: Make the common case fast**_
- Small constants appear often in programs, so supporting them directly saves time.
---
### **Constant Zero**
- Register x0 (zero) is always zero.    
- Useful in many situations:    
    - Copy a register: `add t1, s1, zero`.        
    - Check if a value is zero: `beq t1, zero, target`.  
