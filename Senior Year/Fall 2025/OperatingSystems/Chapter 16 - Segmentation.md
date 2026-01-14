Operating systems
10-06-2025

### The Problem with Base and Bounds
**Why Base and bounds is wasteful:**
- In the simple base and bounds approach, then entire address space must be placed as one continuous block in physical memory
- This means all the empty "free" spaces between the heap and stack also takes up real physical memory
- Even though that space is not being used , its still counting valuable memory resource
- ---
### What is segmentation?
**Basic concept**:
- Segmentation solves the waste problem by breaking the address space into separate pieces called segments
- A segment is just a continuous (connected) chunk of the address space with a specific length
- instead of one big block, we now have separate segments for code, heap, and stack
- each segment can be placed in a different location in physical memory
**Key features**
- Each segment gets its own base and bounds registers
- Segments can grow and shrink independently without affecting each other
- this eliminates the wasted space in the middle of the address space
- ----
### How segments are placed in physical memory
Example setup:
- Code segment: starts at physical address 32K, size 2K
- Heap segment: starts at physical address 34K, size 2K
- Stack segment: starts at physical address 28K, size 2K
- These segments are scattered throughout physical memory instead of one continuous block
- The gaps between them can be used by other programs
- ---
### Address translation with segmentation
**For Code and Heap (Grow forward):**
- Formula: `physical address = offset + base`
- Example: Virtual address 100 in the code segment
	- Offset is 100 (since code starts at virtual address 0)
	- Base is 32K
	- Physical address = 100 + 32K = 32,868
**For heap**:
- Example: Virtual address 4200
	- The heap starts at virtual address 4096 (4K)
	- Offset = 4200 - 4096 = 104
	- Base is 34K
	- Physical address = 104 + 34K = 34,920
- Important Note: You can't just add the virtual address to the base. You must calculate the offset within the segment first.
---
### Segmentation faults
**What causes them**:
- If a program tries to access an address beyond the end of a segment, the hardware detects this is "out of bounds"
- The OS then triggers a segmentation fault error
- Example: If the heap only goes up to 6KB, trying to access 7KB would cause a segmentation fault
- ---
### How the Hardware Knows Which Segment
**Explicit Approach - using segment Bits**:
- The top few bits of the virtual address indicate which segment it belongs to 
- In the Example, a 14 bit address use the top 2 bits for the segment
	- 00 = Code
	- 01 = Heap
	- 10 = Stack
	- 11 = Unused
- The remaining 12 bits give the offset within that segment
- Example: VA 4200 in binary is 01000001101000
	- Top 2 bits: 01 (heap segment)
	- Remaining bits: 000001101000 (offset of 104)
#### Stack segment - special case
**Stack grows backward**:
Unlike code and heap, the stack grows toward lower addresses (backward)
The segment register needs an extra bit to indicate the growth direction
- 1 = grows forward (positive direction)
- 2 = grows backward (negative direction)

**Address translation for stacks**
- Example: VA 15KB
	- Stack starts at VA 16KB (at the top of the address space)
	- Offset = 15KB - 16KB = -1 KB (negative because its below the start)
	- Base is 28K
	- Physical address = 28K - 1K = 27K
- We can think of a stack like a bunker style building where the "ground floor" is at the top and you add floors digging (going) downward
- ---
### Code sharing and protection
**Protection Bits:**
- Each segment has permission bits that control what operations are allowed:
	- Read: can read data from this segment
	- Write: Can modify data in this segment
	- Execute: Can run code from this segment
- Example permissions:
	- Code: Read-Execute (can read and run, but not modify)
	- Heap: Read-Write (can read and modify, but not run as code)
	- Stack: Read-Write (can read and modify, but not run as code)
**Hardware checks**
- Before any memory access, the hardware checks two things:
	1. Is the address within the segment's bounds?
	2. Does the operation match the segments permissions?
- If either check fails, the hardware raises an exception
**Code sharing**:
- Multiple programs can share the same code segment in physical memory 
- For example, if two programs both use the same library, they can point to the same physical code
- This saves memory since the code only needs to be stored once
- Protection bits ensure the code can't be modified
- ---
### Fine-Grained bs Coarse-Grained Segmentation
**Coarse-Grained (what we have been discussing)**:
- Uses just a few large segments (code, heap, stack)
- simple and efficient
- Most common approach
**Fine-Grained**:
- Allows many small segments
- More flexible - you could have dozens or even hundreds of segments
- Requires a segment table in memory to keep track of all segments
- Used in some older systems but less common today
- ---
### External Fragmentation Problem
**The issue**:
- As programs start and stop, they leave behind small gaps of free memory
- These gaps are scattered throughout physical memory
- Even if you have enough total free memory, it might not be in one continuous block
- Example:
	- You have 24LB free total but in three 8KB chunks
	- A program that needs 20KB of continuous memory can't fit
- We can think of it like trying to park a bus in a parking lot that has plenty of total empty space, but only small car-sized spots are available and scattered around.
**Solution: Compaction**
- The OS can rearrange memory by moving segments around to create larger continuous free block if needed
- However, this is expensive because:
	- You must stop the current running processes
	- Copy all of their data to new locations
	- Update all the segment registers with their new base addresses
**Other solutions**:
- Different allocation algorithms (best-fit, worst-fit, first-fit, buddy-algorithm)
- These try to place new segments in smart locations to minimize fragmentation
- None completely eliminate fragmentation, but they can reduce it
- Best fit: Find the smallest gap that fits
- First fit: Use the first gap that's big enough
- Worst fit: Use the largest available gap
---
## Key Takeaway

Segmentation improves on simple base and bounds by dividing the address space into logical pieces that can be placed separately in physical memory. This eliminates wasted space and provides better memory utilization. However, it introduces complexity in address translation and can lead to external fragmentation over time.