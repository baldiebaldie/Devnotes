Computer Architecture
09-10-2025

### **RECAP: The 3 design principles**
1. ***Design principle 1*** : You ideally want to keep things simple and regular. This makes computers faster and cheaper.
2. ***Design principle 2*** : smaller is faster. A smaller decoder is both cheaper and faster.
3. _**Design Principle 3: Make the common case fast**_. Small constants appear often in programs, so supporting them directly saves time.
---
### **Unsigned Binary Integers**
- With **n bits**, the range is **0 to 2ⁿ – 1**.    
- Example with 4 bits:    
    - 0000 = 0        
    - 1011 = 11        
    - 1111 = 15        
- With 64 bits: range is from 0 to 18,446,774,073,709,551,615.
- ---
### **Ones’ Complement**
- Negative numbers are made by flipping all the bits.    
- Range: equal positive and negative values.    
- Problem: there are **two zeros** (+0 and –0).    
- Rarely used today, but seen in old systems and in IPv4 checksums.    
- Example:    
    - 00001010 = +10        
    - 11111111 = –0 (weird duplicate zero)
 ----
### **Two’s Complement (2’s Complement)**
- Most common system today.    
- Negative numbers are made by flipping all bits and **adding 1**.    
- Range: –2ⁿ⁻¹ to +2ⁿ⁻¹ – 1.    
- Example with 32 bits:    
    - 1111…1100 = –4        
    - 0000…0010 = +2        
- With 64 bits: from –9,223,372,036,854,775,808 to +9,223,372,036,854,775,807.   

---

### **Signed Negation**
- To negate a number: **flip bits, add 1**.    
- Example:    
    - +2 = 0000…0010        
    - Flip = 1111…1101        
    - Add 1 = 1111…1110 = –2       

---

## **Sign Extension**
- When increasing bit width, keep the number’s meaning.    
- Positive numbers: extend with 0s.    
- Negative numbers: extend with 1s.    
- Example (8-bit to 16-bit):    
    - +2: 00000010 → 00000000 00000010        
    - –2: 11111110 → 11111111 11111110        

---
### **Hexadecimal**
- Base 16 system. Each hex digit = 4 bits.    
- Digits: 0–9 and A–F.    
- Example: Hex `eca8 6420` = binary `1110 1100 1010 1000 0110 0100 0010 0000`.    
- Easier to read long binary numbers.
---
### Stored Program Computers
- instructions are stored in memory just like data
- Programs can manipulate other programs (like compilers)
- Machine code allows compatibility across machines with the same ISA. This is because most ISAs are standardized
---
### R-format instructions (Register Format where we do arithmetic function)
- Fields:    
    - opcode (operation)        
    - rd (destination register)        
    - rs1, rs2 (source registers)        
    - funct3, funct7 (extra operation codes)        
- Example: `add x9, x20, x21` is stored as a 32-bit binary pattern.
---
## I-format Instructions
- Used for immediate arithmetic and loads.
   - Fields: rs1, rd, opcode, and immediate (constant).
	    Design Principle 4: **Good design requires compromise**.
--- 
## S-format Instructions
- Used for store instructions.
- Fields: rs1 (base), rs2 (source), immediate (offset).
- Immediate is split into two parts to keep rs1 and rs2 aligned in position.
- --
![](../../images/Pasted%20image%2020250915155349.png)

### What each section does and what this means

#### General format
- rs1- Names the source register
- rs2 - Names the second source register
- operation (works with funct 7 and op code)
- rd - destination
- opcode - The field that identifies the main category of the instruction. For example, this tells the computer this is an R-instruction

#### R-format
- funct 7 - 7 bit field which specifies the exact register
- funct 3 - 3 bit field which further specifies the specific 

#### I-format
- immediate: holds constant values that can be used directly

#### S-format
- imm\[11:5]: Top 7 bits of the immediate value. Helps determine memory address
- imm\[4:0]: Bottom 5 vits of the immediate value which combines with the other imm bits to help find the exact memory address.
---
### Producing an Object Module
- An object module is the intermediate result from compiling code
- It contains:
	1. **Header** - describes whats inside
	2. **Text segment** - the actual machine instructions
	3. **Static data segment** – variables that stay for the whole program.    
	4.  **Relocation info** – adjusts addresses when loading.    
	5. **Symbol table** – lists global variables and external references.    
	6. **Debug info** – helps link source code to machine code for debugging.
---
### Linking object Modules
- Combines multiple object modules into a single executable program (.exe file)
- Steps:
	1. Merge code and data segments
	2. Resolve labels (assign addresses)
	3. Patch location-dependent references.
- An example is linking together main.o and math.o to create the final program.
- This is known as *Static linking*
---
### Loading a Program
- The OS loads a program from disk into memory
1. Read header (to know segment size)
2. Create virtual address space
3. Cop text and data into memory
4. Set up arguments on stack
5. Initialize registers (stack pointer, frame pointer)
6. Jump to startup routine --> calls `main`.
7. When `main` finishes, program exist with a system call
---
### Dynamic linking
- Instead of linking everything at once, the program loads libraries only when needed.
- The benefits are that:
	- It saves memory 
	- Always uses the latest library version
- An example is a program using `printf` loads the printf library code at runtime
---
### Basic blocks
- A basic block is a straight sequence of instructions
	- No jumps inside (only at end)
	- No jump targets inside (only at the beginning)
- There is no situation where there is a code flow change midway through
- This is used for compiler optimizations
- Example optimizations are 
	- Removing dead code (unused computations)
	- Reuse repeated expressions (so they computed once)
- --
### Common Compiler Optimization
1. Constant propagation - Replace calculations with constants
	-  Example: `x = 3*4 + y` → `x = 12 + y`.
2. Variable propagation - Replace with known values
	- Example: `X = Y; Z = Y + 5;` → `Z = X + 5`.
3. Strength reduction - Replaces multiplication/division with shifts.
	- Example: `X = Y * 8` → `X = Y << 3`.
4. Loop optimizations –
    - Move loop-invariant code outside.
    - Merge loops.
    - Unroll loops to reduce overhead.
---
### Effect of Compiler Optimization
- Different optimization levels (O1, O2, O3) reduce instruction count and execution time
- An example: Ona  Pentium 4 processor, optimized C code can run at nearly 3x faster than unoptimized code.
- Key Idea: A good algorithm + optimizations = biggest speedup
- NOTHING can fix a bad algorithm