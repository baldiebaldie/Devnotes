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
- Signed bs Unsigned integers: Same bits, different meanings. Like how "FF" could mean 255 if unsigned or -1 if signed.
- Fixed point: Uses some bits for the whole number part and some for the fractional part. Like always having exactly 2 decimal places for money
- Floating point: Like scientific notation - separate the significant digits from the scale factor
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