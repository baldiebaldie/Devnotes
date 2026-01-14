Computer Architecture
09-24-2025

## Introduction to computer arithmetic
**Hardware limitations**
Computers can only represent a fixed number of values based on how many bits they have
- 8-bit: 256 different values (0-255 or -128 to +127)
- 16-bit: 65,536 different values
- 32-bit: about 4.3 billion values
- 64-bit: about 18.4 quintillion values
The same pattern of bits can mean different things depending on how we interpret them. Its like the same sequence of letters could be a word in English or Spanish - the meaning depends on the language we are using.

### Different Number Representations
Computers use several ways to represent numbers:
- Signed bs Unsigned integers: Same bits, different meanings. Like how "0xFF" could mean 255 if unsigned or -1 if signed.
- Fixed point: Uses some bits for the whole number part and some for the fractional part. Like always having exactly 2 decimal places for money
- Floating point: Like scientific notation - separate the significant digits from the scale factorS
- ---
### Hardware Implementation
**Ripple Carry Adder**:
This is the most straightforward way to build an adder - it mimics how you'd add by hand:
- Each bit position has a "full adder" that takes two input buts plus a carry in
- It produces a sum bit and a carry out bit
**Carry lookahead adder**
This solves the speed problem by predicting the carry signals ahead of time
- Instead of waiting for each carry to ripple through, it carries simultaneously
- Uses "Generate" and "Propagate" signals to predict when carries will occur
- Much faster but requires more complex hardware
Think of it like having a traffic control system that changes all the lights at once based on traffic patterns, rather than having each light wait for the previous one. 

---
### What is floating point
- Floating point is a way computers store decimal numbers. The decimal point can "float" to different positions.
- This is just like scientific notation. 
- For example: 
	- 1.101 x 2$^3$
#### IEEE Floating point standard
The IEEE 754 standard tells computers exactly how to store floating point numbers. 
The two main sizes are
- Single point precision: 32 bits total
- Double precision: 64 bits

#### Parts of a floating point number
Every floating point number has 3 parts:
1. Sign Bits
	- 0 is positive 
	- 1 is negative
2. Exponent
	- Single precision uses 8 bits
	- Double precision uses 11 bits
	- The exponent uses a "bias" to handle negative exponents
	- Single precision bias = 127 (8 bits all 1)
	- Double precision bias = 1023 (11 bits all 1)
	- Note there is a hidden 1 at the front that isn't stored
**The formula**: (-1)$^S$ x (1 + Fraction) x 2^(Exponent - Bias)
#### Converting decimal to floating point
**Example:** Convert -3.25 to single point precision
1. Determine sign bit (in this case -3.25 indicates to use 1 since negative)
2. Convert to binary
	- 3 = 0011
	- 0.25 = 0100
	- combined 11.01
3. Normalize (move decimal point so exponent is always 1 digit then decimal)
	- 11.01 = 1.101 x 2$^1$ 
	- The exponent is 1
4. Calculate biased exponent
	- Actual exponent + bias = 1 + 127 = 128
	- 128 in binary is 10000000
5. Get the fraction
	 - After the decimal point: 101
	 - Pad with zeros to fill 23 bits 
 6. Put it all together
	 - 1 10000000 101000000...0000
	 - with the format (sign bit (1b) | exponent (8b) | fraction part (23b))
#### Converting Floating point to decimal
**Example:** Convert 01000010001010000000000000000000 (single point precision) to decimal
1. Break it down
	- 0 10000100 01010000000000000000000
	- 0 is the sign bit 
	- 10000100 is the exponent section
	- 01010000000000000000000 is the fraction section
2. Convert exponent
	- 10000100 is 132
	- 132 - 127 (bias) = 5
	- so the exponent is 2$^5$
3. Build the significand
	- Add the hidden 1: 1.01010000000000000000000
	- 01010000000000000000000 = 0.3125
	- This equals 1 + 0.3125
4. apply the exponent 
	- 1.3125 * 2$^5$ = 42
5. Apply the sign
	- Sign bit = 0 so positive
#### Range and precision
**Single precision**
- Smallest value: approximately ±1.2 × 10⁻³⁸
- Largest value: approximately ±3.4 × 10³⁸
- Precision: about 6-7 decimal digits

**Double Precision:**
- Smallest value: approximately ±2.2 × 10⁻³⁰⁸
- Largest value: approximately ±1.8 × 10³⁰⁸
- Precision: about 15-16 decimal digits

---
### Special Values
Zero:
- Exponent = all zeros
- Fraction = all zeros
- Can be +0 or -0
Denormalized Numbers:
- Very tiny numbers near zero
- Exponent = all zeros
- Fraction != 0
- Represents invalid operations like 0/0
**Infinity:**

- Exponent = all ones
- Fraction = all zeros
- Can be +infinity or -infinity

**NaN (Not a Number):**

- Exponent = all ones
- Fraction != zero
- Represents invalid operations like 0/0

#### Floating Point Addition

**Example: 9.999 × 10¹ + 1.610 × 10⁻¹**

**Step 1:** Align the decimal points

- Shift the smaller number: 0.016 × 10¹

**Step 2:** Add the significands

- 9.999 + 0.016 = 10.015

**Step 3:** Normalize

- 10.015 × 10¹ = 1.0015 × 10²

**Step 4:** Round if needed

- Round to 4 digits: 1.002 × 10²

#### Floating Point Multiplication

**Example: 1.110 × 10¹⁰ × 9.200 × 10⁻⁵**

**Step 1:** Add exponents

- 10 + (-5) = 5

**Step 2:** Multiply significands

- 1.110 × 9.200 = 10.212

**Step 3:** Normalize

- 10.212 × 10⁵ = 1.0212 × 10⁶

**Step 4:** Round

- 1.021 × 10⁶

**Step 5:** Determine sign

- Positive × positive = positive

## Guard, Round, and Sticky Bits (GRS)

These are extra bits that help make floating point math more accurate.

**Analogy:** Think of measuring ingredients for baking. If you only use whole cups, you lose precision. But if you also track half-cups and quarter-cups, your measurements are more accurate.

The GRS bits work the same way. They keep track of extra precision during calculations so the final answer is more accurate.

#### Why Floating Point Matters

Floating point allows computers to work with very large numbers (like distances between stars) and very small numbers (like the size of atoms) using the same format.

The tradeoff is that floating point numbers aren't always perfectly accurate. They're approximations. This is why sometimes 0.1 + 0.2 doesn't exactly equal 0.3 on a computer.

For money calculations, this is bad! That's why banks use fixed point or special decimal types instead.

---