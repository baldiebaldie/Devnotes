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