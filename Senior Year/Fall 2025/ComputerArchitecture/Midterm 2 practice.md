Computer Architecture
10-15-2025

# Computer Architecture Practice Problems and Examples

## UNIT 2: Instructions and Assembly Language

---

### **Example 1: Arithmetic Example**

**Problem (word for word):**

```
C code:
f = (g + h) + (i + j);

Compiled RISC-V (pseudo*)code:
add t0, g, h // temp t0 = g + h
add t1, i, j // temp t1 = i + j
add f, t0, t1 // f = t0 + t1
```

**Description:** This problem shows how to turn a math expression from C code into assembly instructions.

**Purpose:** To learn how high-level code gets broken down into simple assembly steps.

**Key Words:**

- **Compiled**: Changed from one programming language to another
- **Temporary (temp)**: A storage spot used for a short time during calculations
- **Pseudo code**: Fake code that shows the idea but won't actually run

---

### **Example 2: Register Operand Example**

**Problem (word for word):**

```
C code:
f = (g + h) - (i + j);

f, …, j in x19, x20, …, x23

Compiled RISC-V code:
add x5, x20, x21  add t0, s4, s5
add x6, x22, x23  add t1, s6, s7
sub x19, x5, x6  sub s3, t0, t1
```

**Description:** This example shows the same type of problem but with subtraction and using actual register names.

**Purpose:** To practice converting C code to assembly using real register locations.

**Key Words:**

- **Register**: A small, fast storage spot inside the processor
- **Operand**: A value that an instruction works with
- **Subtraction (sub)**: Taking one number away from another

---

### **Example 3: Memory Operand Example**

**Problem (word for word):**

```
C code:
A[12] = h + A[8];

h in x21, base address of A in x22

Compiled RISC-V code:
Index 8 requires offset of 64
8 bytes per doubleword
ld x9, 64(x22) // 8 bytes * index 8
add x9, x21, x9
sd x9, 96(x22)
```

**Description:** This problem works with arrays stored in memory instead of just registers.

**Purpose:** To understand how to load data from memory, do math on it, and store it back.

**Key Words:**

- **Array**: A list of values stored together in memory
- **Base address**: The starting location of an array in memory
- **Offset**: How many spots away from the start you need to go
- **Load (ld)**: Getting data from memory into a register
- **Store (sd)**: Putting data from a register back into memory
- **Doubleword**: A 64-bit piece of data

---

### **Example 4: Try it out! (Assembly Code Sample)**

**Problem (word for word):**

```assembly
.data
a: .word 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15
b: .dword 0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15
.text
la s0, a
la s1, b
li s2, 5
lw t0, 32(s0)
add t0, t0, s2
sw t0, 48(s0)
ld t0, 64(s1)
add t0, t0, s2
sd t0, 96(s1)
```

**Description:** This is actual runnable code that shows the difference between words and doublewords in memory.

**Purpose:** To see how word size affects the offset calculations when accessing arrays.

**Key Words:**

- **Word**: A 32-bit piece of data
- **Load address (la)**: Put a memory address into a register
- **Load immediate (li)**: Put a number directly into a register
- **Load word (lw)**: Get a 32-bit value from memory
- **Store word (sw)**: Put a 32-bit value into memory

---

### **Example 5: Compiling If Statements**

**Problem (word for word):**

```
C code:
if (i==j) f = g+h;
else f = g-h;

f, g, … in x19, x20, …

Compiled RISC-V code:
bne x22, x23, Else
add x19, x20, x21
beq x0,x0,Exit // unconditional
Else: sub x19, x20, x21
Exit: …
```

**Description:** This shows how if-else statements become branch instructions in assembly.

**Purpose:** To learn how the computer makes decisions and jumps to different code sections.

**Key Words:**

- **Branch**: Jump to a different part of the code
- **Branch if not equal (bne)**: Jump if two values are different
- **Branch if equal (beq)**: Jump if two values are the same
- **Label**: A name marking a spot in the code
- **Unconditional**: Always happens, no matter what

---

### **Example 6: Compiling Loop Statements**

**Problem (word for word):**

```
C code:
while (save[i] == k)
    i += 1;

i in x22, k in x24, address of save in x25

Compiled RISC-V code:
Loop: slli x10, x22, 3
      add x10, x10, x25
      ld x9, 0(x10)
      bne x9, x24, Exit
      addi x22, x22, 1
      beq x0, x0, Loop
Exit: …
```

**Description:** This example shows how a while loop becomes repeating instructions with a branch.

**Purpose:** To understand how loops work at the assembly level.

**Key Words:**

- **Loop**: Code that repeats multiple times
- **Shift left logical immediate (slli)**: Multiply by powers of 2 by shifting bits left
- **Add immediate (addi)**: Add a number directly to a register

---

### **Example 7: More Conditional Operations**

**Problem (word for word):**

```
Example
if (a > b) a += 1;

a in x22, b in x23

bge x23, x22, Exit // branch if b >= a
addi x22, x22, 1
Exit:
```

**Description:** This shows comparison operations for greater than or less than.

**Purpose:** To learn how to compare numbers and make decisions based on the comparison.

**Key Words:**

- **Branch if greater or equal (bge)**: Jump if the first value is bigger or the same
- **Comparison**: Checking how two values relate to each other

---

### **Example 8: Signed vs. Unsigned**

**Problem (word for word):**

```
Example
x22 = 1111 1111 1111 1111 1111 1111 1111 1111
x23 = 0000 0000 0000 0000 0000 0000 0000 0001

x22 < x23 // signed
–1 < +1

x22 > x23 // unsigned
+4,294,967,295 > +1
```

**Description:** This shows how the same bit pattern means different things if you treat it as signed or unsigned.

**Purpose:** To understand that the meaning of bits depends on how you interpret them.

**Key Words:**

- **Signed**: Numbers that can be positive or negative
- **Unsigned**: Numbers that are only positive
- **Interpretation**: How you understand the meaning of something

---

### **Example 9: Clearing an Array**

**Problem (word for word):**

```
clear1(int array[], int size) {
    int i;
    for (i = 0; i < size; i += 1)
        array[i] = 0;
}

clear2(int *array, int size) {
    int *p;
    for (p = &array[0]; p < &array[size]; p = p + 1)
        *p = 0;
}
```

**Assembly for clear1:**

```
li x5,0
loop1:
    slli x6,x5,2
    add x7,x10,x6
    sd x0,0(x7)
    addi x5,x5,1
    blt x5,x11,loop1
```

**Assembly for clear2:**

```
mv x5,x10
slli x6,x11,2
add x7,x10,x6
loop2:
    sd x0,0(x5)
    addi x5,x5,4
    bltu x5,x7,loop2
```

**Description:** This compares two ways to clear an array: using array indexing versus using pointers.

**Purpose:** To see how different C coding styles create different assembly code.

**Key Words:**

- **Pointer**: A variable that holds a memory address
- **Array indexing**: Accessing array elements using their position number
- **Move (mv)**: Copy a value from one register to another
- **Branch if less than (blt)**: Jump if the first value is smaller
- **Branch if less than unsigned (bltu)**: Jump if the first value is smaller (treating both as positive)

---

### **Example 10: Leaf Procedure Example**

**Problem (word for word):**

```
C code:
long long int leaf_example (
    long long int g, long long int h,
    long long int i, long long int j) {
    long long int f;
    f = (g + h) - (i + j);
    return f;
}

Arguments g, …, j in x10, …, x13
f in x20
temporaries x5, x6

RISC-V code:
leaf_example:
    addi sp,sp,-24
    sd x5,16(sp)
    sd x6,8(sp)
    sd x20,0(sp)
    add x5,x10,x11
    add x6,x12,x13
    sub x20,x5,x6
    addi x10,x20,0
    ld x20,0(sp)
    ld x6,8(sp)
    ld x5,16(sp)
    addi sp,sp,24
    jalr x0,0(x1)
```

**Description:** This shows a complete function that doesn't call any other functions.

**Purpose:** To learn how functions save and restore registers using the stack.

**Key Words:**

- **Procedure/Function**: A piece of code that does a specific job
- **Leaf procedure**: A function that doesn't call other functions
- **Stack**: A memory area for temporary storage
- **Stack pointer (sp)**: Register that points to the top of the stack
- **Jump and link register (jalr)**: Return from a function
- **Caller**: The code that calls a function
- **Callee**: The function that gets called

---

### **Example 11: Non-Leaf Procedure Example**

**Problem (word for word):**

```
C code:
long long int fact (long long int n)
{
    if (n < 1) return 1;
    else return n * fact(n - 1);
}

Argument n in x10
Result in x10

RISC-V code:
fact:
    addi sp,sp,-16
    sd x1,8(sp)
    sd x10,0(sp)
    addi x5,x10,-1
    bge x5,x0,L1
    addi x10,x0,1
    addi sp,sp,16
    jalr x0,0(x1)
L1: addi x10,x10,-1
    jal x1,fact
    addi x6,x10,0
    ld x10,0(sp)
    ld x1,8(sp)
    addi sp,sp,16
    mul x10,x10,x6
    jalr x0,0(x1)
```

**Description:** This is a recursive function (one that calls itself) to calculate factorial.

**Purpose:** To understand how functions that call other functions must save their return address.

**Key Words:**

- **Non-leaf procedure**: A function that calls other functions
- **Recursive**: A function that calls itself
- **Return address**: The spot in code to go back to after a function finishes
- **Jump and link (jal)**: Call a function and save where to return
- **Factorial**: Multiplying all whole numbers up to a given number

---

### **Example 12: String Copy Example**

**Problem (word for word):**

```
C code:
void strcpy (char x[], char y[])
{
    size_t i;
    i = 0;
    while ((x[i]=y[i])!='\0')
        i += 1;
}

RISC-V code:
strcpy:
    addi sp,sp,-8
    sd x19,0(sp)
    add x19,x0,x0
L1: add x5,x19,x10
    lbu x6,0(x5)
    add x7,x19,x10
    sb x6,0(x7)
    beq x6,x0,L2
    addi x19,x19,1
    jal x0,L1
L2: ld x19,0(sp)
    addi sp,sp,8
    jalr x0,0(x1)
```

**Description:** This copies one string to another by copying each character until hitting the null terminator.

**Purpose:** To work with strings and character data in assembly.

**Key Words:**

- **String**: A sequence of characters
- **Null-terminated**: A string that ends with a zero byte
- **Load byte unsigned (lbu)**: Get one byte from memory without sign extension
- **Store byte (sb)**: Put one byte into memory
- **Character**: A single letter, number, or symbol

---

### **Example 13: C Sort Example - Swap Procedure**

**Problem (word for word):**

```
void swap(long long int v[], long long int k)
{
    long long int temp;
    temp = v[k];
    v[k] = v[k+1];
    v[k+1] = temp;
}

v in x10, k in x11, temp in x5

swap:
    slli x6,x11,3
    add x6,x10,x6
    ld x5,0(x6)
    ld x7,8(x6)
    sd x7,0(x6)
    sd x5,8(x6)
    jalr x0,0(x1)
```

**Description:** This function swaps two adjacent elements in an array.

**Purpose:** To see a simple helper function used in sorting.

**Key Words:**

- **Swap**: Exchange the positions of two values
- **Adjacent**: Next to each other
- **Temporary variable**: A storage spot for holding a value during swapping

---

### **Example 14: The Sort Procedure**

**Problem (word for word):**

```
void sort (long long int v[], size_t n)
{
    size_t i, j;
    for (i = 0; i < n; i += 1) {
        for (j = i – 1;
             j >= 0 && v[j] > v[j + 1];
             j -= 1) {
            swap(v,j);
        }
    }
}

v in x10, n in x11, i in x19, j in x20
```

**Description:** This is a bubble sort algorithm with nested loops.

**Purpose:** To see a complete sorting function that calls another function.

**Key Words:**

- **Nested loops**: Loops inside other loops
- **Bubble sort**: A sorting method that compares adjacent items and swaps them
- **Sorted**: Arranged in order

---

## UNIT 3: Number Representations and Arithmetic

---

### **Example 15: Unsigned Binary Integers**

**Problem (word for word):**

```
Example
0000 0000 … 0000 1011₂ = 0 + … + 1×2³ + 0×2² +1×2¹ +1×2⁰
                        = 0 + … + 8 + 0 + 2 + 1 = 11₁₀
```

**Description:** This shows how to convert a binary number to decimal by adding up powers of 2.

**Purpose:** To understand how unsigned binary numbers work.

**Key Words:**

- **Binary**: The base-2 number system using only 0 and 1
- **Decimal**: The base-10 number system we use every day
- **Bit**: A single binary digit (0 or 1)
- **Power of 2**: Multiplying 2 by itself a certain number of times
- **Unsigned**: Only positive numbers, no negative values

---

### **Example 16: Ones' Complement**

**Problem (word for word):**

```
Examples…
00001010 = +10
10001010 = -(01110101) = -117
11111111 = -(00000000) = 0
```

**Description:** This shows an old way to represent negative numbers by flipping all the bits.

**Purpose:** To know about ones' complement even though it's rarely used.

**Key Words:**

- **Ones' complement**: A number system where you flip all bits to make a negative
- **Flip**: Change 0 to 1 and 1 to 0
- **Negative**: Less than zero

---

### **Example 17: 2's Complement Signed Integers**

**Problem (word for word):**

```
Example
1111 1111 … 1111 1100₂
= –1×2³¹ + 1×2³⁰ + … + 1×2² +0×2¹ +0×2⁰
= –2,147,483,648 + 2,147,483,644 = –4₁₀
```

**Description:** This shows how to interpret a 2's complement negative number by making the leftmost bit negative.

**Purpose:** To understand the modern way computers store negative numbers.

**Key Words:**

- **2's complement**: The standard way to represent signed numbers in computers
- **Sign bit**: The leftmost bit that tells if a number is positive or negative
- **Most significant bit (MSB)**: The leftmost bit with the highest value

---

### **Example 18: Signed Negation**

**Problem (word for word):**

```
Example: negate +2
+2 = 0000 0000 … 0010two
–2 = 1111 1111 … 1101two + 1
   = 1111 1111 … 1110two
```

**Description:** This shows how to make a number negative by flipping all bits and adding 1.

**Purpose:** To learn the process for finding the negative of a number in 2's complement.

**Key Words:**

- **Negate**: Change a positive to negative or negative to positive
- **Complement**: Flip all the bits

---

### **Example 19: Sign Extension**

**Problem (word for word):**

```
Examples: 8-bit to 16-bit
+2: 0000 0010 => 0000 0000 0000 0010
–2: 1111 1110 => 1111 1111 1111 1110
```

**Description:** This shows how to make a number bigger by copying the sign bit to the left.

**Purpose:** To preserve the value when converting to more bits.

**Key Words:**

- **Sign extension**: Copying the sign bit when making a number use more bits
- **Preserve**: Keep something the same

---

### **Example 20: Hexadecimal**

**Problem (word for word):**

```
Example: eca8 6420
1110 1100 1010 1000 0110 0100 0010 0000
```

**Description:** This shows how to convert between hexadecimal and binary.

**Purpose:** To work with hex numbers as a shorthand for binary.

**Key Words:**

- **Hexadecimal (hex)**: Base-16 number system using 0-9 and A-F
- **Compact**: Short and efficient

---

### **Example 21: R-format Example**

**Problem (word for word):**

```
add x9,x20,x21

0000 0001 0101 1010 0000 0100 1011 0011two = 22,676,659₁₀ = 0x015A04B3
```

**Description:** This shows how an assembly instruction gets encoded as a 32-bit binary number.

**Purpose:** To see what instructions look like as actual machine code.

**Key Words:**

- **Machine code**: Binary numbers that the processor actually executes
- **Opcode**: The part of an instruction that says what operation to do
- **Encoding**: Converting instructions into binary
- **Format**: The pattern of how instruction bits are arranged

---

### **Example 22: Integer Addition**

**Problem (word for word):**

```
Example: 7 + 6
```

(Shows binary addition with overflow conditions)

**Description:** This shows binary addition and when overflow can happen.

**Purpose:** To understand how addition works and its limitations.

**Key Words:**

- **Overflow**: When a result is too big to fit in the available bits
- **Carry**: A bit that moves to the next column during addition

---

### **Example 23: Integer Subtraction**

**Problem (word for word):**

```
Example: 7 – 6 = 7 + (–6)
+7: 0000 0000 … 0000 0111
–6: 1111 1111 … 1111 1010
+1: 0000 0000 … 0000 0001
```

**Description:** This shows that subtraction is done by adding the negative.

**Purpose:** To see how computers use addition circuits for subtraction too.

**Key Words:**

- **Subtraction**: Taking one number away from another
- **Negation**: Making a positive negative or negative positive

---

### **Example 24: Fixed Point**

**Problem (word for word):**

```
Examples
10.11 → 2.75
1100.001 → 12.125
0101.101 → (2² + 2⁰).(2⁻¹+2⁻³) = (4+1).(1/2+1/8) = 5.625
```

**Description:** This shows how to work with binary numbers that have a fractional part.

**Purpose:** To represent decimal numbers with a fixed number of bits for the fraction.

**Key Words:**

- **Fixed point**: Numbers with a set position for the decimal point
- **Binary point**: The dot separating whole and fractional parts in binary
- **Fraction**: The part of a number after the decimal point
- **Precision**: How exact a number can be

---

### **Example 25: Floating-Point Example (Representing -0.75)**

**Problem (word for word):**

```
Represent –0.75
–0.75 = (–1)¹ × 1.1₂ × 2⁻¹
S = 1
Fraction = 1000…00₂
Exponent = –1 + Bias
Single: –1 + 127 = 126 = 01111110₂
Double: –1 + 1023 = 1022 = 01111111110₂
Single: 1011111101000…00
Double: 1011111111101000…00
```

**Description:** This shows step-by-step how to encode a decimal number in IEEE floating point format.

**Purpose:** To practice converting decimal to floating point.

**Key Words:**

- **Floating point**: A way to represent very large or very small numbers
- **IEEE Standard**: The official rules for floating point representation
- **Exponent**: The power of 2 that scales the number
- **Mantissa/Fraction**: The significant digits of the number
- **Bias**: A number added to the exponent to make it unsigned
- **Normalized**: Adjusted so there's one digit before the binary point

---

### **Example 26: Floating-Point Example (What number is...)**

**Problem (word for word):**

```
What number is represented by the single-precision float
11000000101000…00

S = 1
Fraction = 01000…00₂
Exponent = 10000001₂ = 129
x = (–1)¹ × (1 + .01₂) × 2⁽¹²⁹ ⁻ ¹²⁷⁾
  = (–1) × 1.25 × 2²
  = –5.0
```

**Description:** This shows the reverse process of decoding a binary floating point number to decimal.

**Purpose:** To practice converting floating point to decimal.

**Key Words:** (Same as above)

---

### **Example 27: Practice Problems (Converting to FP)**

**Problem (word for word):**

```
Convert 3.625 to single-precision FP
Provide the solution in hex

Solution:
S = 0
Binary → 11.101 → 1.1101 x 2¹
Exponent = 1 + bias = 128
Mantissa = 1101
0_10000000_11010….0
0100_0000_0110_1000….0
0x40680000
```

**Description:** This is a practice problem for converting a decimal to hex floating point.

**Purpose:** To test your understanding of the floating point conversion process.

**Key Words:** (Same as floating point examples above)

---

### **Example 28: Practice Problems (Converting to FP - Negative)**

**Problem (word for word):**

```
Convert -12.75 to single-precision FP
Provide the solution in hex

Solution:
S = 1
Binary → 1100.11 → 1.10011 x 2³
Exponent = 3 + bias = 130
Mantissa = 10011
1_10000010_100110….0
1100_0001_0100_1100…0
0xC14C0000
```

**Description:** Another conversion practice problem with a negative number.

**Purpose:** To practice the full conversion process including sign bit.

**Key Words:** (Same as floating point examples above)

---

### **Example 29: Practice Problems (Hex to Decimal)**

**Problem (word for word):**

```
What is the value of 0xc1f80000 (assume single-precision FP)

Solution:
1100_0001_1111_1000_0….0
1_10000011_11110….0
S = 1
Exponent = 131 – bias = 4
Mantissa = .1111
-1.1111 x 2⁴ = -31
```

**Description:** This converts a hex floating point number back to decimal.

**Purpose:** To practice decoding floating point representations.

**Key Words:** (Same as floating point examples above)

---

### **Example 30: Practice Problems (Another Hex to Decimal)**

**Problem (word for word):**

```
What is the value of 0x40a00000 (assume single-precision FP)

Solution:
0100_0000_1010_0…0
0_10000001_0100…0
S = 0
Exponent = 129 – bias = 2
Mantissa = .01
1.01 x 2² = 101 = 5
```

**Description:** Another decoding practice problem.

**Purpose:** More practice with hex to decimal floating point conversion.

**Key Words:** (Same as floating point examples above)

---

### **Example 31: Floating-Point Addition**

**Problem (word for word):**

```
Consider a 4-digit decimal example
9.999 × 10¹ + 1.610 × 10⁻¹

1. Align decimal points
   9.999 × 10¹ + 0.016 × 10¹
2. Add significands
   9.999 × 10¹ + 0.016 × 10¹ = 10.015 × 10¹
3. Normalize result & check for over/underflow
   1.0015 × 10²
4. Round and renormalize if necessary
   1.002 × 10²
```

**Description:** This shows the steps for adding two floating point numbers.

**Purpose:** To understand how floating point addition is more complex than integer addition.

**Key Words:**

- **Align**: Line up the decimal points by adjusting exponents
- **Significand**: The main digits of the number
- **Normalize**: Adjust so there's one non-zero digit before the point
- **Round**: Adjust to fit within precision limits

---

### **Example 32: Floating-Point Addition (Binary)**

**Problem (word for word):**

```
Now consider a 4-digit binary example
1.000₂ × 2⁻¹ + –1.110₂ × 2⁻² (0.5 + –0.4375)

1. Align binary points
   1.000₂ × 2⁻¹ + –0.111₂ × 2⁻¹
2. Add significands
   1.000₂ × 2⁻¹ + –0.111₂ × 2⁻¹ = 0.001₂ × 2⁻¹
3. Normalize result & check for over/underflow
   1.000₂ × 2⁻⁴, with no over/underflow
4. Round and renormalize if necessary
   1.000₂ × 2⁻⁴ (no change) = 0.0625
```

**Description:** The same addition process but with binary numbers.

**Purpose:** To see how floating point addition works in binary.

**Key Words:** (Same as previous addition example)

---

### **Example 33: Floating-Point Multiplication**

**Problem (word for word):**

```
Consider a 4-digit decimal example
1.110 × 10¹⁰ × 9.200 × 10⁻⁵

1. Add exponents
   New exponent = 10 + –5 = 5
2. Multiply significands
   1.110 × 9.200 = 10.212 → 10.212 × 10⁵
3. Normalize result & check for over/underflow
   1.0212 × 10⁶
4. Round and renormalize if necessary
   1.021 × 10⁶
5. Determine sign of result from signs of operands
   +1.021 × 10⁶
```

**Description:** This shows the steps for multiplying floating point numbers.

**Purpose:** To understand floating point multiplication.

**Key Words:**

- **Multiply**: Combine two numbers by repeated addition
- **Sign**: Whether a number is positive or negative

---

### **Example 34: Floating-Point Multiplication (Binary)**

**Problem (word for word):**

```
Now consider a 4-digit binary example
1.000₂ × 2⁻¹ × –1.110₂ × 2⁻² (0.5 × –0.4375)

1. Add exponents
   Unbiased: –1 + –2 = –3
   Biased: (–1 + 127) + (–2 + 127) = –3 + 254 – 127 = –3 + 127
2. Multiply significands
   1.000₂ × 1.110₂ = 1.110₂ → 1.110₂ × 2⁻³
3. Normalize result & check for over/underflow
   1.110₂ × 2⁻³ (no change) with no over/underflow
4. Round and renormalize if necessary
   1.110₂ × 2⁻³ (no change)
5. Determine sign: + × – → –
   –1.110₂ × 2⁻³ = –0.21875
```

**Description:** The same multiplication process but in binary.

**Purpose:** To see floating point multiplication with binary numbers.

**Key Words:** (Same as previous multiplication example)

---

### **Example 35: FP Example: °F to °C**

**Problem (word for word):** (Shows assembly code for temperature conversion using floating point)

**Description:** This is a real program that converts Fahrenheit to Celsius using floating point instructions.

**Purpose:** To see how floating point instructions are used in actual programs.

**Key Words:**

- **Floating point register**: A special register just for decimal numbers
- **Single precision**: 32-bit floating point
- **Double precision**: 64-bit floating point

---

### **Example 36: Examples with GRS**

**Problem (word for word):**

```
Consider with 3 bits after bp
```
### **Example 36: Examples with GRS (Guard, Round, Sticky bits) - Problem 1**

**Problem (word for word):**

```
Consider with 3 bits after bp
Infinite Precision:
1.000 x 2⁵ - 1.001 x 2¹
1.000 0000 x 2⁵ - 0.000 1001 x 2⁵
0.111 0111 x 2⁵
1.110 1110 x 2⁴
1.111 x 2⁴

With GRS:
1.000 000 x 2⁵ - 0.000 101 x 2⁵
0.111 011 x 2⁵
1.110 11 x 2⁴
1.111 x 2⁴

With (G/R)S:
1.000 00 x 2⁵ - 0.000 11 x 2⁵
0.111 01 x 2⁵
1.110 1 x 2⁴
1.11? x 2⁴
```

**Description:** This shows how extra precision bits help get more accurate results in floating point subtraction.

**Purpose:** To see why guard, round, and sticky bits matter for accuracy.

**Key Words:**

- **Guard bit**: An extra bit kept during calculations for better accuracy
- **Round bit**: Another extra bit to help with rounding decisions
- **Sticky bit**: A bit that remembers if any 1s were shifted off the right
- **Precision**: How accurate a number representation is

---

### **Example 37: Examples with GRS - Problem 2**

**Problem (word for word):**

```
Consider with 3 bits after bp
Infinite Precision:
1.000 x 2⁵ - 1.111 x 2¹
1.000 0000 x 2⁵ - 0.000 1111 x 2⁵
0.111 0001 x 2⁵
1.110 001 x 2⁴
1.110 x 2⁴

With GRS:
1.000 000 x 2⁵ - 0.000 111 x 2⁵
0.111 001 x 2⁵
1.110 01 x 2⁴
1.110 x 2⁴

With (G/R)S:
1.000 00 x 2⁵ - 0.000 11 x 2⁵
0.111 01 x 2⁵
1.110 1 x 2⁴
1.11? x 2⁴
```

**Description:** Another example showing how guard, round, and sticky bits improve accuracy.

**Purpose:** To practice understanding how extra precision bits work.

**Key Words:** (Same as Example 36)

---

### **Example 38: Multiplication Hardware Steps**

**Problem (word for word):**

```
1000 × 1001

Length of product is the sum of operand lengths
multiplicand: 1000
multiplier: 1001
product: 1001000

Step-by-step hardware progression shown:
Initially 0 / 1000 / 1001 / 0
Then 1000 / 10000 / 0100 / 1000
Then 1000 / 10000 / 100000 / 0100 / 0010 / 01000
Then 1000 / 10000 / 100000 / 1000000 / 0100 / 0010 / 0001 / 01000 / 001000
Final: 1001000
```

**Description:** This shows how multiplication happens step-by-step in hardware with shifting and adding.

**Purpose:** To understand the algorithm computers use to multiply numbers.

**Key Words:**

- **Multiplicand**: The number being multiplied
- **Multiplier**: The number you multiply by
- **Product**: The result of multiplication
- **Partial product**: An intermediate result during multiplication
- **Shift**: Move bits left or right

---

### **Example 39: Division**

**Problem (word for word):**

```
Long division approach:
      1001
1000)1001010
    -1000
       10
      101
     1010
    -1000
       10

n-bit operands yield n-bit quotient and remainder
quotient
dividend
remainder
divisor
```

**Description:** This shows long division in binary using the same process as decimal division.

**Purpose:** To understand how computers divide numbers.

**Key Words:**

- **Division**: Splitting a number into equal parts
- **Dividend**: The number being divided
- **Divisor**: The number you divide by
- **Quotient**: The result of division
- **Remainder**: What's left over after division
- **Restoring division**: A method where you add back if the subtraction goes negative

---

### **Example 40: Right Shift and Division**

**Problem (word for word):**

```
Example
–5 / 4
11111011₂ >> 2 = 11111110₂ = –2
Rounds toward –∞

c.f. 11111011₂ >>> 2 = 00111110₂ = +62
```

**Description:** This shows that right shift divides by powers of 2 but handles signed numbers differently than unsigned.

**Purpose:** To understand the difference between arithmetic and logical right shift.

**Key Words:**

- **Right shift**: Move all bits to the right
- **Arithmetic right shift**: Copies the sign bit when shifting right
- **Logical right shift**: Fills with zeros when shifting right
- **Divide by 2**: What happens with each right shift

---

### **Example 41: FP Example: Array Multiplication (Matrix Multiply)**

**Problem (word for word):**

```
C = C + A × B
All 32 × 32 matrices, 64-bit double-precision elements

C code:
void mm (double c[][], double a[][], double b[][]) {
    size_t i, j, k;
    for (i = 0; i < 32; i = i + 1)
        for (j = 0; j < 32; j = j + 1)
            for (k = 0; k < 32; k = k + 1)
                c[i][j] = c[i][j] + a[i][k] * b[k][j];
}

Addresses of c, a, b in x10, x11, x12, and i, j, k in x5, x6, x7
```

(Full RISC-V assembly code shown in slides)

**Description:** This is a complete example of matrix multiplication using nested loops and floating point operations.

**Purpose:** To see how complex math operations work with multiple dimensions of data.

**Key Words:**

- **Matrix**: A rectangular grid of numbers
- **Nested loop**: A loop inside another loop
- **Two-dimensional array**: A grid or table of values
- **Matrix multiplication**: A special way to multiply two grids of numbers

---

### **Example 42: Synchronization Example 1 - Atomic Swap**

**Problem (word for word):**

```
Example 1: atomic swap (to test/set lock variable)
again: lr.d x10,(x20)
       sc.d x11,(x20),x23 // X11 = status
       bne x11,x0,again // branch if store failed
       addi x23,x10,0 // X23 = loaded value
```

**Description:** This shows how to safely swap values when multiple processors might access the same memory.

**Purpose:** To understand synchronization in multi-processor systems.

**Key Words:**

- **Synchronization**: Making sure multiple processors work together correctly
- **Atomic operation**: An operation that completes fully without interruption
- **Load reserved (lr)**: Load a value and reserve the memory location
- **Store conditional (sc)**: Store only if the reservation is still valid
- **Lock**: A mechanism to control access to shared resources

---

### **Example 43: Synchronization Example 2 - Lock**

**Problem (word for word):**

```
Example 2: lock
addi x12,x0,1 // copy locked value
again: lr.d x10,(x20) // read lock
       bne x10,x0,again // check if it is 0 yet
       sc.d x11,(x20),x12 // attempt to store
       bne x11,x0,again // branch if fails

Unlock:
sd x0,0(x20) // free lock
```

**Description:** This shows how to create a lock mechanism so only one processor can access shared data at a time.

**Purpose:** To learn how computers prevent conflicts when sharing memory.

**Key Words:**

- **Lock**: A way to prevent others from accessing something
- **Unlock**: Release the lock so others can use it
- **Spin lock**: Continuously checking until a lock becomes available

---

## Summary Note

These examples cover the key concepts you'll need for your midterm:

**Unit 2 Topics:**

- Converting C code to assembly
- Using registers and memory
- Branching and loops
- Functions and the stack
- Arrays and pointers

**Unit 3 Topics:**

- Number representations (unsigned, signed, floating point)
- Binary and hex conversions
- Arithmetic operations and their hardware
- Overflow and precision issues
- Fixed point and floating point formats

**Study Tips:**

- Practice converting between decimal, binary, and hex
- Work through the assembly code examples line by line
- Understand when overflow happens
- Know the IEEE floating point format parts (sign, exponent, mantissa)
- Remember that computers have limited precision

# RISC-V Instruction Format Deep Dive

## The Big Picture

Every RISC-V instruction is **exactly 32 bits** (4 bytes). These 32 bits are divided into different **fields** (sections) that tell the processor what to do. Different instruction types have different layouts.

---

## The Six Main Instruction Formats

RISC-V uses 6 formats: **R, I, S, SB, U, UJ**

Let me break down each one:

---

## R-Format (Register Format)

**Used for:** Register-to-register operations (like `add`, `sub`, `and`, `or`)

**Layout:**

```
[funct7: 7 bits][rs2: 5 bits][rs1: 5 bits][funct3: 3 bits][rd: 5 bits][opcode: 7 bits]
  31-25          24-20         19-15        14-12          11-7         6-0
```

**Field Meanings:**

- **opcode (7 bits)**: The main operation code. Tells what general category of instruction this is.
    
    - Example: `0110011` means R-type arithmetic operation
- **rd (5 bits)**: **Destination register**. Where the result goes.
    
    - 5 bits can represent 32 different registers (x0-x31)
    - Example: `01001` = x9
- **funct3 (3 bits)**: **Function code** (part 1). Helps specify the exact operation.
    
    - Works with opcode to narrow down what instruction this is
    - Example: `000` might mean ADD
- **rs1 (5 bits)**: **First source register**. First input value.
    
    - Example: `10100` = x20
- **rs2 (5 bits)**: **Second source register**. Second input value.
    
    - Example: `10101` = x21
- **funct7 (7 bits)**: **Function code** (part 2). Further specifies the operation.
    
    - Example: `0000000` means ADD, `0100000` means SUB

**Example: `add x9, x20, x21`**

```
funct7=0000000 | rs2=10101 | rs1=10100 | funct3=000 | rd=01001 | opcode=0110011
```

**Why it's designed this way:**

- rs1, rs2, and rd are always in the same position across all formats (makes hardware simpler)
- Uses all 32 bits for maximum information

---

## I-Format (Immediate Format)

**Used for:** Instructions with one register and one constant (like `addi`, `lw`, `ld`)

**Layout:**

```
[immediate: 12 bits][rs1: 5 bits][funct3: 3 bits][rd: 5 bits][opcode: 7 bits]
  31-20              19-15        14-12          11-7         6-0
```

**Field Meanings:**

- **opcode (7 bits)**: Main operation code
    
    - Example: `0010011` = immediate arithmetic
    - Example: `0000011` = load instruction
- **rd (5 bits)**: Destination register (same position as R-format)
    
- **funct3 (3 bits)**: Function code (same position as R-format)
    
    - Example: `000` = ADDI
    - Example: `011` = LD (load doubleword)
- **rs1 (5 bits)**: Source register (same position as R-format)
    
- **immediate (12 bits)**: The constant value
    
    - Can represent -2048 to +2047 (signed 12-bit number)
    - **Sign-extended** to 64 bits when used
    - This means if bit 11 is 1, all upper bits become 1

**Example: `addi x22, x22, 4`**

```
immediate=000000000100 | rs1=10110 | funct3=000 | rd=10110 | opcode=0010011
```

**For load instructions: `ld x9, 64(x22)`**

- The immediate (64) is the **offset** from the base address in x22
- rd (x9) gets the loaded value
- rs1 (x22) holds the base address

**Why it's designed this way:**

- 12 bits covers most common constants
- Keeps rs1 and rd in same positions as R-format

---

## S-Format (Store Format)

**Used for:** Store instructions (like `sw`, `sd`, `sb`)

**Layout:**

```
[imm[11:5]: 7 bits][rs2: 5 bits][rs1: 5 bits][funct3: 3 bits][imm[4:0]: 5 bits][opcode: 7 bits]
  31-25              24-20         19-15        14-12          11-7              6-0
```

**Field Meanings:**

- **opcode (7 bits)**: Operation code
    
    - Example: `0100011` = store instruction
- **imm[4:0] (5 bits)**: **Lower 5 bits** of the immediate (offset)
    
- **funct3 (3 bits)**: Specifies store size
    
    - `000` = SB (store byte)
    - `001` = SH (store halfword)
    - `011` = SD (store doubleword)
- **rs1 (5 bits)**: Base address register (same position as always)
    
- **rs2 (5 bits)**: **Source register** - the value to store
    
    - Note: This is in the rs2 position, same as R-format!
- **imm[11:5] (7 bits)**: **Upper 7 bits** of the immediate (offset)
    

**Why the immediate is split:** The immediate is broken into two pieces to keep rs1 and rs2 in the **same positions** as R-format and I-format. This makes the hardware decoder simpler.

**Example: `sd x9, 96(x22)`**

```
imm[11:5]=0000011 | rs2=01001 | rs1=10110 | funct3=011 | imm[4:0]=00000 | opcode=0100011
```

The offset 96 in binary is `000001100000` (12 bits)

- Upper 7 bits: `0000011`
- Lower 5 bits: `00000`

**Key difference from I-format:**

- I-format: loads INTO rd
- S-format: stores FROM rs2 (no destination register needed)

---

## SB-Format (Branch Format)

**Used for:** Conditional branches (like `beq`, `bne`, `blt`, `bge`)

**Layout:**

```
[imm[12]: 1 bit][imm[10:5]: 6 bits][rs2: 5 bits][rs1: 5 bits][funct3: 3 bits][imm[4:1]: 4 bits][imm[11]: 1 bit][opcode: 7 bits]
  31              30-25               24-20        19-15        14-12          11-8              7                 6-0
```

**Field Meanings:**

- **opcode (7 bits)**: Operation code
    
    - Example: `1100011` = branch instruction
- **imm[11] (1 bit)**: Bit 11 of the offset (in position 7)
    
- **imm[4:1] (4 bits)**: Bits 4-1 of the offset (in positions 11-8)
    
- **funct3 (3 bits)**: Branch type
    
    - `000` = BEQ (branch if equal)
    - `001` = BNE (branch if not equal)
    - `100` = BLT (branch if less than)
- **rs1 (5 bits)**: First register to compare
    
- **rs2 (5 bits)**: Second register to compare
    
- **imm[10:5] (6 bits)**: Bits 10-5 of the offset
    
- **imm[12] (1 bit)**: Sign bit (bit 12 of offset)
    

**Important notes:**

- The immediate represents a **PC-relative offset**
- The offset is in **multiples of 2 bytes** (bit 0 is always 0, not stored)
- Total range: -4096 to +4094 bytes from current PC
- The immediate is **scrambled** to keep rs1 and rs2 in standard positions

**Example: `beq x5, x6, Loop`** If Loop is 8 bytes back, the offset is -8

- Actual immediate value: -8 ÷ 2 = -4 = `1111111111100` (13 bits in 2's complement)
- These bits get scattered into the instruction format

**Why it's designed this way:**

- Keeps comparison registers (rs1, rs2) in same spots
- Multiplying offset by 2 doubles the branch range
- Branch targets are always even addresses (instructions are 4-byte aligned anyway)

---

## U-Format (Upper Immediate Format)

**Used for:** Large immediates (like `lui`, `auipc`)

**Layout:**

```
[immediate: 20 bits][rd: 5 bits][opcode: 7 bits]
  31-12              11-7         6-0
```

**Field Meanings:**

- **opcode (7 bits)**: Operation code
    
    - `0110111` = LUI (load upper immediate)
    - `0010111` = AUIPC (add upper immediate to PC)
- **rd (5 bits)**: Destination register
    
- **immediate (20 bits)**: A 20-bit constant
    

**What LUI does:**

```assembly
lui x19, 976
```

- Takes the 20-bit immediate (976)
- Puts it in bits [31:12] of x19
- Sets bits [11:0] to zero
- Sign-extends bit 31 to bits [63:32]

**Example:** 976 = `0000 0000 0011 1101 0000` (20 bits) After `lui x19, 976`:

```
x19 = 0000...0000 0000 0000 0011 1101 0000 0000 0000 0000
                 [------976------] [----zeros----]
```

**Building a 32-bit constant:**

```assembly
lui x19, 976      # Load upper 20 bits
addi x19, x19, 128  # Add lower 12 bits
```

This builds: `(976 << 12) + 128`

**Why it's designed this way:**

- Gives you 20 bits for large constants
- Combined with ADDI's 12 bits, you can build any 32-bit value
- Simple format with no source registers

---

## UJ-Format (Unconditional Jump Format)

**Used for:** Jumps with large offsets (like `jal`)

**Layout:**

```
[imm[20]: 1 bit][imm[10:1]: 10 bits][imm[11]: 1 bit][imm[19:12]: 8 bits][rd: 5 bits][opcode: 7 bits]
  31              30-21               20              19-12                11-7         6-0
```

**Field Meanings:**

- **opcode (7 bits)**: Operation code
    
    - `1101111` = JAL (jump and link)
- **rd (5 bits)**: Where to save the return address (usually x1)
    
- **immediate (20 bits total, scrambled)**: PC-relative jump offset
    
    - **imm[20]**: Sign bit
    - **imm[19:12]**: Bits 19-12 of offset
    - **imm[11]**: Bit 11 of offset
    - **imm[10:1]**: Bits 10-1 of offset
    - Bit 0 is always 0 (not stored, implied)

**What JAL does:**

```assembly
jal x1, Function
```

1. Save PC+4 (return address) into x1
2. Jump to (PC + offset)

**Offset calculation:**

- The 20-bit immediate is multiplied by 2 (bit 0 assumed to be 0)
- Range: -1,048,576 to +1,048,574 bytes

**Why it's designed this way:**

- 20 bits gives much larger jump range than branches
- Stores return address for function calls
- Bits scrambled but rd stays in normal position

---

## Why Multiple Formats?

**Design Principle: Good design demands good compromises**

Each format is optimized for its use case:

1. **R-format**: Three registers, needs room for all three
2. **I-format**: Two registers + small constant, most common
3. **S-format**: Two registers + offset, but no destination
4. **SB-format**: Two registers + bigger offset for branches
5. **U-format**: One register + huge constant
6. **UJ-format**: One register + huge jump offset

**Common features across formats:**

- Opcode always in bits [6:0]
- rd always in bits [11:7] (when present)
- rs1 always in bits [19:15] (when present)
- rs2 always in bits [24:20] (when present)
- funct3 always in bits [14:12] (when present)

This **regularity** makes the hardware simpler even though there are multiple formats.

---

## How The Processor Uses These Fields

When the processor fetches an instruction:

1. **Decode the opcode** (bits 6-0) - "What category is this?"
2. **Check funct3 and funct7** (if present) - "What specific operation?"
3. **Extract register numbers** - "Which registers do I need?"
4. **Extract immediate** (if present) - "Is there a constant? Reassemble it from scattered bits"
5. **Execute** - Do the operation
6. **Write result** - Store in rd (if applicable)

---

## Practice Problem

**Decode this instruction: `0x015A04B3`**

Convert to binary:

```
0000 0001 0101 1010 0000 0100 1011 0011
```

Break into fields (this is R-format based on opcode):

```
funct7  rs2    rs1    f3  rd     opcode
0000000 10101  10100  000 01001  0110011
   0      21     20    0    9     51
```

**Translation:**

- opcode = `0110011` → R-type arithmetic
- funct3 = `000` → ADD operation
- funct7 = `0000000` → confirms ADD (not SUB)
- rd = `01001` = x9
- rs1 = `10100` = x20
- rs2 = `10101` = x21

**Result:** `add x9, x20, x21`

---

## Summary

**Key Takeaways:**

- Instructions are 32-bit patterns divided into fields
- Field positions stay consistent to simplify hardware
- Different formats optimize for different needs
- Immediates are stored IN the instruction, not separate memory
- The hardware decoder looks at opcode first, then other fields
- Understanding formats helps you read machine code and understand instruction limitations