
### What is Paging?
**The core idea**
- Paging solves the problems we saw this segmentation by dividing memory into fixed-size chunks called pages
- Both the virtual address space and physical memory are divided into equal-sized pieces
- In the virtual address space, these pieces are called pages
- In physical memory, these pieces are called page frames
- Unlike segmentation (which had variable-sized segments) all pages are the same size
**The goals of paging**:
- Remove the requirement that an address space must be stored continuously in physical memory
- Eliminate external fragmentation (those annoying gaps between segments)
- Allow segments to grow as needed w/o worrying about running into each other.
**Comparison to Segmentation**
- Segmentation uses variable-sized logical segments (code, stack, heap)
- Paging uses fixed-sized pages that can be scattered anywhere in physical memory

### Advantages of Paging
**Flexibility**
- The OS does not need to make assumptions about how the stack will grow 
- Pages can go anywhere in physical memory, so there's no pressure to keep things together
**Simplicity**
- Because all pages are the same size, managing free space is much easier
- when you need a page, just grab any free one from the list
- No need to search for a chunk of the "right size" like with segmentation

**Example Mapping:**
- Virtual Page 0 → Physical Frame 3
- Virtual Page 1 → Physical Frame 7
- Virtual Page 2 → Physical Frame 5
- Virtual Page 3 → Physical Frame 2

### How Address Translation Works
**Components of a virtual address**
1. VPN (Virtual Page Number) - which page we want
2. Offset - where within the page we want to access
**Translation Process**
- Example: Virtual address 21 in a 64-byte address space
- Split the address into VPN and offset
- Look up the VPN in the page table to get the PFN
- Combine the PFN with the offset to create the physical address
- The offset stays the same - only the page/frame number changes

**Step-by-step for address 21:**
- Binary: 010101
- Top 2 bits (01) = VPN 1
- Bottom 4 bits (0101) = offset 5
- Page table says VPN 1 is at PFN 7
- Physical address = PFN 7 + offset 5
- --
### Page Table Entries (PTEs)
**What's in a PTE**?
- Each entry in the page table contains more than just the physical frame number. It also has several flag bits
**Valid Bits**
- Indicates whether this translation is valid
- If invalid, the page isn't being used by the process
**Protection Bits**
- Controls whether the page can be read, written to, or executed
- Same concept as with segmentation
Present Bit:
- Indicates whether the page is currently in physical memory or has been swapped out to disk
- Important for when we have more virtual memory than physical memory
**Dirty bit:**
- Tracks whether the page has been modified since being loaded into memory
- Helps the OS know if it needs to save changes when swapping the page out.
**Reference/Accessed Bit**:
- Indicates that the page has been accessed recently
- Helps the OS make decisions about which pages to keep in memory
---
### Pre-Process page tables
**Important concept:**
- Each process has its own separate page table
- Different processes can have the same virtual addresses that map to different physical locations
- This is how multiple processes can all think multiple processes can all think they start at address 0
**What happens on context switch**
- The OS saves the old page table base register value
- The OS loads the new process's page table base register
- Now all address translations use the new process's page table
- Like switching between different translation dictionaries when you switch between languages. 
### The Page Table Size Problem
**Page Table Size Problem**
- Example calculation for 32 bit address space with 4KB pages:
	- 32 bits total, 12 bits offset (because 4KB = 2^12)
	- That leaves 20 bits for VPN
	- Number of pages = 2^20 about 1 million pages
	- If each page table entry is 4 bytes, the page table alone is 4MB per process!
	- With 100 processes running, that's 400MB just for page tables
**Formula to Calculate Page Table Size:**
- Page table size = (Number of PTEs) × (Size of each PTE)
- Number of PTEs = 2^(bits for VPN)
- Bits for VPN = Total address bits - Bits for offset
- Bits for offset = log₂(page size)
**The Problem:**
- Page tables take up a lot of memory themselves
- This is one of the main disadvantages of simple linear page tables
- --
### Where are Page Tables Stored?
**In memory**
- Page tables are too large to fit in special hardware registers (unlike with segmentation)
- They're stored in regular memory
- The hardware only stores the base address of the page table in a special register called the PTBR (Page Table Base Register)
**Finding a Page Table Entry**:
- Use the formula: PTE Address = PTBR + (VPN x sizeof(PTE))
- This tells you where in memory to find the translation for a specific virtual page
**The Performance Problem**
- Every memory access now requires TWO memory accesses:
	  1. One to fetch the page table entry
	  2. One to actually access the data you wanted
- This effectively doubles the time for memory access!
---
### Memory Access Example

**Simple Loop Example:**
```c
for (i = 0; i < 1000; i++)
    array[i] = 0;
```

**What Actually Happens:** For each instruction in the loop:
1. Fetch the instruction (requires looking up its page in the page table)
2. Execute the instruction
3. If it accesses memory (like writing to the array), look up that page in the page table too
4. Actually write to memory
**Memory Access Count:**
- 10 memory accesses per loop iteration
- 4 instruction fetches
- 1 data write
- 5 page table lookups (one for each of the above)
**The Overhead:**
- Paging adds significant overhead because of all these extra page table lookups
- This is why modern systems use a TLB (Translation Lookaside Buffer), which we'll learn about in future lectures
- --
### Advantages of Paging
**No External Fragmentation:**
- Any page can fit in any frame
- No need to search for contiguous space
- Allocation and deallocation are fast and simple
- Use a bitmap to track free frames

**Easy to Swap to Disk:**
- Page sizes typically match disk block sizes
- Can move pages between memory and disk easily
- Processes can run even when some pages are on disk
- The present bit in the PTE tracks whether a page is in memory or on disk

**Sharing is Possible:**
- Multiple processes can share pages by having their page tables point to the same physical frame
- Useful for sharing code libraries between programs
---
### Disadvantages of Paging

**Internal Fragmentation:**
- A process might not need a full page, but it gets one anyway
- The unused portion of the page is wasted
- Larger pages mean more waste
- Example: If pages are 4KB and you only need 100 bytes, you waste 3,996 bytes

**Performance Overhead:**
- Each memory access requires an extra memory access to look up the page table
- This makes programs run slower
- Solution: TLB (Translation Lookaside Buffer) - a hardware cache for page table entries

**Page Tables Take Up Space:**
- Linear page tables need entries for every possible page, even unused ones
- This can waste a lot of memory
- A 32-bit address space might need millions of page table entries per process
---
### Steps for Address Translation

**The Complete Process:**

1. Extract VPN and offset from virtual address (cheap - just bit manipulation)
2. Calculate PTE address using PTBR and VPN (cheap - simple arithmetic)
3. Read the PTE from memory (expensive - memory access)
4. Extract the PFN from the PTE (cheap - bit manipulation)
5. Build physical address by combining PFN and offset (cheap - bit operations)
6. Access memory with the physical address (expensive - memory access)

**Two Expensive Steps:**
- Reading the PTE from memory
- Actually accessing the data
- These memory accesses are the bottleneck in paging performance
---
### Key Takeaway

Paging solves the external fragmentation problem of segmentation by using fixed-size pages that can be placed anywhere in physical memory. However, it introduces new challenges including large page tables that consume memory and performance overhead from the extra memory accesses needed to look up translations. The next lectures will cover solutions to these problems, particularly the TLB for speeding up address translation.