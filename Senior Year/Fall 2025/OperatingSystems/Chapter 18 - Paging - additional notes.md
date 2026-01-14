Issue with continuous memory is fragmentation

Internal fragmentation - assigned space not fully used by process
External fragmentation - we do not have enough continuous space for a requested processes but that amount does exist

### What is paging
- We take virtual memory and split it into pages
- We take our physical memory and split it into frames
		`Page size = frameSize`
- Now we can store each page to a frame hence mapping virtual memory directly to physical memory without any holes (so no fragmentation)
- Frame size
	- Always power of 2 to make translation of logical address to page # and offset easier
	
--- 
### How to implement paging?
- Now we have each address partitioned into two parts
	- Page number 
		- Indexes into the page table
	- Page offset
		- Indexes into the actual page itself. (where in the page you are sitting)
- The page table holds the ID of the frame number which gets us to the right frame in memory

Similarly there is one for physical addresses that is partitioned by...
- Frame number
	- The number that lets us go from page table to the correct frame in memory
- Offset

Example:
![](../../../-images/Pasted%20image%2020251013124924.png)


---
---
---
---
## Notes from me reading the chapter
- TLB is part of the chips memory management unit 
- HARDWARE
- Simply a hardware cache of popular virtual to physical translations
- Could be renamed address translation cache 

ON HIT
1. Grab the VPN
2. If TLB has VPN, TLB hit
3. Extract PFN and concatenate that onto the offset of VA and form the OA

ON MISS
1. Grab the VPN
2. TLB does not have VPN, TLB miss
3. Update TLB with the translation
4. Then run retry instruction (to get TLB hit)

Note: When a miss occurs it is very costly due to the additional memory access.
- TLB is found near the processing core so its very fast
- **TLB misses lead to more memory accesses**

Lets say we are storing data in memory like this 
![](../../../-images/Pasted%20image%2020251020121141.png)

- If we were to iterate through each entry.
- a[0] would be a TLB miss (then added to TLB cache) and then a[1] and a[2] would both be hits because the VPN (page 06) was added to the TLB thus being quicker to access.
- a[3] would have a TLB miss but a[4] - a[6] would be TLB hit for the same reason
- 3 misses out of 10 entries. 3/10 = 30% miss rate thus 70% hit rate
- This idea is called spatial locality, since the data is close to each other in memory its faster to access
- also if the page size was larger (more individual entries can fit per page) hit rate would be higher
- If we ran loop again it would be all hits. This is because of temporal locality, quick re-references of memory items in time

### who handles TLB miss?
- Both hardware and OS
**Hardware**
- Old days with CISC computers hardware knew where the page tables were located (Page table base register (PTBR)) and their format. On a miss the hardware would walk to the page table and find the correct entry and then grab it and throw it into the TLB
**OS**
- On TLB miss now an exception is raised which pauses current instruction stream and then raises to kernel mode and jumps to trap handler which then can throw the page into the TLB.
- The return from trap needs to be different. On TLB miss the hardware must resume executions at the instruction that caused the trap rather than the instructions after the trap. This acts as a retry which lets the instructions run again
- Advantage of this approach is flexibility: The OS can use any data structure it wants to implement the page table, without necessitating hardware change and simplicity.

### TLB contents
A typical TLB entry usually looks like
- VPN | PFN | other bits
- Hardware searches VPN and PFN in parallel to see if there is a match
- Valid bit: Tells whether the entry has valid translation 
- Protection bit: Determines how a page can be accessed (marks read or write on TLB)

### Difference between TLB valid bit and Page table valid bit
- TLB valid bit: Tells if entry in the TLB is usable.
- Page table valid bit: Tells you if page in memory is loaded and accessible
- Overall main difference is that the TLB valid bits are about cached translations while page table valid bits are about whether actual memory pages exist and are available
### How to handle context switch
Problem: Switching to another process might cause problems if that process tries and accesses old TLB data
- Solution 1: Flush TLB. Set all TLB valid bits of old process to 0 basically deleting all old entries
- Solution 2: ASID (address space identifier). Each process gets its own ASID so the TLB entries can be differentiated between processes so when you switch back you do not have to go through all of the misses again
### If we run out of TLB space but want new entry, which one do we get rid of?
- LRU (least recently used)
- Random