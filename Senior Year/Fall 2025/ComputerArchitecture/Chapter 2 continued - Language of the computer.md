Computer Architecture
09-10-2025

### **RECAP: The 3 design principles**
1. ***Design principle 1*** : You ideally want to keep things simple and regular. This makes computers faster and cheaper.
2. ***Design principle 2*** : smaller is faster. A smaller decoder is both cheaper and faster.
3. _**Design Principle 3: Make the common case fast**_. Small constants appear often in programs, so supporting them directly saves time.
4. *Design Principle 4:* **Good design demands good compromises**
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
![](../../../-images/Pasted%20image%2020250915155349.png)

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
- --
### Compiling loops
```c
while (save[i] == k) {
	i +=1;
}
```
Becomes assembly code with:
- Array indexing calculation (multiply index by 8, add to base address)
- Load from memory
- Compare values
- Branch back to loop start or exit
---
### Arrays vs pointers
**Array access**
using arrays requires:
- multiplying the index by element size
- adding to the base address
You can think of it as going to room 100 and then count 5 doors down

---
### Example: Clearing an array
Two ways to clear an array:
1. **Array version**: Uses index `i`, multiplies by element size each time  
2. **Pointer version**: Uses pointer `p` just adds element size each time

The pointer version can be faster, but modern compilers are smart enough to optimize array code to be just as fast.

---
### Memory layout
##### How programs are organized in memory
We can think of memory like a tall apartment building:
- Text segments - the actual program instructions
- static data - Global variables that never change size
- Heap - Dynamic memory
- Stack - Function calls and local variables
![](../../../-images/Pasted%20image%2020250922160107.png)
The global pointer (x3) points to static data so you can access it quickly

---
### Procedure calls
stack frames: When you call a function
- local variables go on the stack
- return address gets saved
- arguments get passed
- When function returns, everything gets cleaned up
FIRST IN FIRST OUT.
So if you were to write a function this is what happens under the hood,\:
- Put arguments in registers x10-x17
- Jump to the function (jal instruction)
- Function allocates space for local variables
- Function does its work
- Function puts result in a register
- Function cleans up its space
- Function returns to caller (jalr instruction)
---
### Procedure call instructions
#### Leaf procedures 
Leaf procedures do not call other functions. They're simpler because they only need to:
- Save any registers they use the caller might need later
- Do their own work
- Restore saved registers
- Return
``` R
leaf_example:
    addi sp,sp,-24        # Make room on stack
    sd   x5,16(sp)        # Save registers we will use
    sd   x6,8(sp)
    sd   x20,0(sp)
    
    add  x5,x10,x11       # Do the actual work
    add  x6,x12,x13
    sub  x20,x5,x6
    
    addi x10,x20,0        # Put result in the return-register
    
    ld   x20,0(sp)        # Restore saved registers
    ld   x6,8(sp)
    ld   x5,16(sp)
    addi sp,sp,24         # Clean up stack
    jalr x0,0(x1)         # Return
```

#### Non-Leaf procedures
Non-leaf procedures call other functions, so they are more complex. They must save:
- Their return address (so they know where to go back)
- Any arguments and variables they need after the function call
``` r
fact:
    addi sp,sp,-16        # Make room for return address and n
    sd   x1,8(sp)         # Save return address
    sd   x10,0(sp)        # Save n
    
    addi x5,x10,-1        # Check if n < 1
    bge  x5,x0,L1         # If n >= 1, go to recursive case
    
    addi x10,x0,1         # Base case: return 1
    addi sp,sp,16         # Clean up stack
    jalr x0,0(x1)         # Return
    
L1: addi x10,x10,-1       # n = n - 1
    jal  x1,fact          # Recursive call
    
    addi x6,x10,0         # Save result of fact(n-1)
    ld   x10,0(sp)        # Restore original n
    ld   x1,8(sp)         # Restore return address
    addi sp,sp,16         # Clean up stack
    
    mul  x10,x10,x6       # n * fact(n-1)
    jalr x0,0(x1)         # Return
```

---
### Character Data and Strings
#### Character Encoding
Computers can store text with ASCII: 
-  128 characters 
#### Note you can use other sizes for load and save instructions
i.e.
- `lb` - Load byte (8 bits), extend sign
- `lbu` - Load byte unsigned (8 bits), fill with zeros
- `lh` - Load halfword (16 bits), extend sign
- `lw` - Load word (32 bits), extend sign

##### Example: Loading a character
```r
lbu  x5,0(x10)           # Load unsigned byte (character)
beq  x5,x0,end_string    # Check if null terminator
```
---
### Large Constants and Addressing

#### 32-bit Constants
Most numbers in programs are small and fit into 12 bits. For larger numbers, RISC-V uses two instructions. 
The instruction:
	**`lui` (Load Upper Immediate)**: Loads 20 bits into the upper part of a register **`addi`**: Adds the lower 12 bits
**Example: Loading a large number**
```r
lui  x19,976              # Load upper 20 bits
addi x19,x19,128          # Add lower 12 bits
```

### Branch and Jump addressing
Branches use PC-Relative addressing: The target is calculated as current location + offset. This works well because most branches go to nearby locations.

Jumps can go farther using 20-bit immediate, or anywhere using two instructions with `lui` and `jalr`.

So if you have a huge jump instruction you can chain them together 

### RISC-V Addressing Modes summary
1. Immediate: Value is in the instruction itself.
2. Register: Value is in register
3. Base: Memory Address = register + offset
4. PC-relative: Address = PC + offset (for branches)

#### Instruction set extensions
RISC-V is modular. The base instruction set (RV 641) converts basic operations. Extension add specialized features:
- **M**: Multiply, divide, and remainder operations
- **A**: Atomic operations for multi-processor synchronization
- **F**: Single-precision floating point
- **D**: Double-precision floating point
- **C**: Compressed 16-bit instructions (saves memory)