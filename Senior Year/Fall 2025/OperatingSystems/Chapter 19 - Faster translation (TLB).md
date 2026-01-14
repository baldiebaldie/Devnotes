### The problem with basic paging
Remember:
- Every memory access require two memory accesses:
	- One to read the page table entry
	- One to access the actual data
- This makes everything twice as slow
### What is a TLB?
**TLB = Translation lookaside Buffer**
- We can think of this like a cheat sheet for address translation
 **Key Characteristics**:
 - A small, fast hardware cache inside the CPU's Memory Management Unit
 - Stores recent Virtual-to-physical address translations
 - Much faster than accessing the page table in memory 
Analogy: The page table is like a dictionary with every word. The TLB is like a sticky note with the 10 words you use most often. You check the sticky note first because its faster

#### How the TLB Works
**When you need to translate an address**
1. Check the TLB first (Very fast)
2. If found (TLB hit): Use that translation immediately
3. If not found (TLB miss): Go to the page table in memory (slow), then add it to TLB

**TLB is "fully associative"**
- Any translation can be stored anywhere in the TLB
- Hardware searches ALL entries at the same time (in parallel)
- This parallel search makes it very fast
#### TLB entry contents
Each TLB entry contains:
- VPN (Virtual Page number): The page you are looking for
- PFN (Page Frame number): Where it actually is in physical memory
- Valid bit: Is this entry actually usable?
- Protection bits: Can you read, write, or execute this page. 
- ASID (Address Space Identifier): Which process does this belong to?
- Dirty bit: Has this page been modified?
TLB Size: Typically 32, 64, or 128 entries (Much smaller than page tables)


### Important: TLB Valid Bit bs Page Table Valid Bit
These mean DIFFERENT things:
**Page Table valid bit = 0**:
- That means this page doesn't exist or cannot be accessed
- The OS will kill the process via segmentation fault
**TLB Valid bit = 0:**
- This TLB slot is just empty
- No big deal, just means we need to look in the page table


### TLB Algorithm - on hit
1. Extract VPN from virtual address
2. Look up VPN in TLB
3. If found then TLB Hit
	- Check protection bits
	- If access is allowed:
		- Extract PFN from TLB entry
		- Build the physical address (PFN + offset)
		- Access memory
	- If access is not allowed:
		- Raise protection error
The key: When you get a TLB hit, you skip the slow memory access to the page table
### TLB Algorithm - on miss
1.  Extract VPN from virtual address
2. Look up VPN in TLB
3. If not found then TLB Miss:
	- Calculate page table entry address
	- Read PTE from memory (Slow)
	- Check if page is valid
	- Check if access is allowed
	- Insert translation into TLB
	- Retry the instruction
The miss is expensive because you have to access memory for the page table.

#### Code example
``` c
int sum = 0; 
for(i = 0; i < 10; i++) {
	sum += a[i]; 
}
```
**Setup:**
- Array starts at address 100
- Page size: 16 bits
- Each integer: 4 bytes
- So 4 integers fir per page
**What happens**:
- a[0], a[1], a[2] are on page 6  -> TLB miss on a[0] then 2 hits
- a[3], a[4], a[5], a[6] are on page 7 -> TLB miss on a[3], then 3 hits
- a[7], a[8], a[9] are on page 8 -> TLB miss on a[7] and then 2 hits
**Result**: 3 misses, 7 hits = 70% hit rate

#### Improving TLB performance
`TLB Reach = Number of TLB entries x Page Size`
- This tells you how much memory you can access with just the TLB
**To improve hit rate with a fixed TLB Size**
- You must increase page size
- Have fewer unique pages needed to access the same amount of memory
- Have each TLB entry cover more memory
**Example:**
- 64 TLB entries x 4KB pages = 256KB reach
- 64 TLB entries x 8KB pages = 512KB reach (this is better)
The tradeoff: Larger pages means more internal fragmentation (which is wasted space)

#### Locality of reference
**Spatial Locality:**
- You will soon access nearby addresses
- Example: Looping through an array
- TLB helps: One entry covers many nearby addresses on the same page
**Temporal Locality**:
- You will soon access the same address again
- Example: Accessing a loop counter repeatedly 
- TLB helps: The translation stays in the TLB for reuse
We can think of spatial locality as "Taking a whole paragraph when you look up a line in a book in case you need more from that paragraph". Temporal locality is like keeping your bookmark on that page where you looked up the line in case you need to go back to it.

#### TLB Replacement policies
When the TLB is full and you need to add a new entry, which one do you remove?

LRU (Least recently used)
- Remove the entry that hasn't been used for the longest time
- Takes advantage of temporal locality
- If something was used recently, it will probably be used again soon
Randomly:
- Remove a randomly chosen entry
- Simpler to implement
- Sometimes works almost as well as LRU
#### The context switch problem
The problem: Different processes use the same virtual address but they map to different physical addresses.
Example:
- Process A: VPN 10 -> PFN 100
- Process B: VPN 10 -> PFN 170
Both want VPN 10, but they should get different physical frames! If we do not do something process B might access process A's Memory

##### Solution 1: Flush TLB on context switch
Simple approach:
- When switching process, mark all TLB entries as invalid
- The new process starts with an empty TLB
##### Solution 2: Address Space Identifier (ASID)
Better approach:
- Add an ASID field to each TLB entry
- Each process gets a unique ASID (like a mini process ID)
- When looking up translations, must match both VPN and ASID

**Example TLB with ASID:**

```
VPN  PFN  Valid  Prot  ASID
10   100    1    rwx    1    (Process A's translation)
10   170    1    rwx    2    (Process B's translation)
```

Now both processes can have translations in the TLB at the same time! The hardware knows which one to use based on the ASID.
#### Sharing Pages Between Processes
Sometimes we WANT two processes to access the same physical memory.
**Example: Shared library code**
- Process 1 maps physical frame 101 to its VPN 10
- Process 2 maps physical frame 101 to its VPN 50
- Both access the same physical memory (101) but through different virtual addresses
**TLB entries:**
```
VPN  PFN  Valid  Prot  ASID
10   101    1    rwx    1    (Process 1)
50   101    1    rwx    2    (Process 2)
```
Both entries point to PFN 101! This saves memory because we only need one copy of the code.
#### Address Translation with TLB: Complete Process
**For each memory reference:**
1. Extract VPN and offset from virtual address
2. Search TLB for VPN (and matching ASID)
**If TLB HIT:**
- 3a. Extract PFN from TLB entry
- 4a. Build physical address (PFN + offset)
- 5a. Access memory with physical address
**If TLB MISS:**
- 3b. Calculate page table entry address
- 4b. Read PTE from memory, add to TLB, retry instruction
The TLB turns the two-memory-access process into a one-memory-access process (when you get a hit).
#### Who Handles TLB Misses?
**Hardware-Managed TLB (Intel x86):**
- The hardware automatically handles misses
- Hardware knows where page tables are in memory
- Hardware "walks" the page table to find the translation
- Hardware updates the TLB
- Hardware retries the instruction
- Faster but less flexible
**Software-Managed TLB (MIPS, RISC-V):**
- On a miss, hardware raises an exception
- OS trap handler runs to handle the miss
- OS code looks up the translation
- OS updates the TLB
- OS returns and hardware retries
- Slower but more flexible (OS can use any page table structure)
- ---
## TLB Summary
**Key points:**
1. **TLBs solve the performance problem of paging**
    - Without TLB: 2 memory accesses per reference
    - With TLB: 1 memory access per reference (on hit)
2. **TLB performance depends on workload**
    - Sequential access: great (spatial locality)
    - Repeated random access: can be good (temporal locality)
    - Truly random access: poor
3. **Context switches are challenging**
    - Can flush TLB (simple but slow)
    - Can use ASID (better performance)
4. **TLB is managed by hardware or OS**
    - Hardware-managed: faster but less flexible
    - Software-managed: slower but more flexible

**The big idea:** A small hardware cache (TLB) makes paging practical by eliminating most page table lookups.