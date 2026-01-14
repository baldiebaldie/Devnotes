Operating systems
11-03-2025

I am just going to start watching the videos (from textbook) and take notes on them for each chapter that will be included in the exam. 
- Note that will be chapters 13-16, 18-22, 26-28, 30
---
### Chapter 16 - Segmentation 
https://www.youtube.com/watch?v=I_LKYizRduE

With base and bounds registers OS can relocate processes to different parts of physical memory
- Issue: There can sometimes be large gaps in memory between two bits of memory which are left unused 
- Dynamic relocation w/ a pair of base and bound registers to virtualize memory is wasteful
- (Just drilling it in) : Issue with base and bounds is we have these large virtual address spaces with chunks of unused space which is wasteful
- Internal fragmentation: Putting space aside that will never be used (marking it as used even though its not)
- Address spaces on these processes have to be less than what is on disk
2 problem with base and bounds
- Allocating memory that can be used for something else
- limited to having address spaces which is smaller that the amount of physical memory that is available
Segmentation : instead of blindly allocating the entire address space, we make segments of memory instead.
- We will have base and bound register for each of these segments.
- The process still has access to the entire virtual address space. It think it has more memory than it actually has.
- The main benefit is you can have many many more processes running due to there being more efficient use of the address spaces
- Each segment is fully rearrangeable
For physical translation it is just 

- Segmentation fault occurs when you try to do a translation to an area that is outside the scope of the segment 
How to find address using segments
- Explicit approach
	- Lets say we have 14 bit address space
	- bits 13-12 indicate which segment (00 code, 01 heap, 11 stack)
	- bits 11-0 indicate the offset 
	- If the offset happens to go outside the range of the segment, throw segmentation fault
- Implicit approach
	- Hardware can guess which address belongs to what. 
	- For example if something comes from the stack pointer, hardware can guess the data is sitting within the stack.

Stack stuff 
- Stack grows backwards (grows upwards)
- Because of this we need to have a negative offset
- requires help from the hardware which has an extra bit that indicates which direction the offset must go (Negative growth support)
- To get it you subtract offset from the maximum segment size. Then add negative offset from beginning of the segment

Because of the way that segmentation works you can allow multiple processes to share the same area of physical memory
- Note needs protection bit (determines read/write privileges for each segment)

What responsibility does the OS have?
- OS has responsibility of keeping track of base and size register (IN PCB)
- Conducting the change of registers on context switching
- allow processes to grow and ask for more space
- Has to manage the different parts of physical memory 

External fragmentation
- Different size chunks of free memory between segments which can cause problems. You would ideally want to move all of the free space together.
---
### Chapter 17  - Free Space management (lowkey wont be on exam kappa)
https://www.youtube.com/watch?v=npiYFhuh1WU

Fragmentation 
- Free space must be managed to avoid fragmentation
- Hard when free space are in variable sizes

Low level mechanisms
- Splitting and Coalescing
- Tracking the size of allocated regions
- Building a simple list inside the free space to keep track of the free space

Splitting
- Finding a free chunk of memory and splitting it into two 
- Then when there is a request for memory then throw it into one of those free chunks
![](../../../-images/Pasted%20image%2020251103122129.png)
- Example here. We need 15 bits but there is no continuous chunk of 15 bits to store in memory. So we split into 2 and now can store a part in each chunk of free memory 

Coalescing
- Opposite of splitting
- You can take multiple close free chunks and merge them together to use. 

Tracking size of allocated regions
- The interface `free (void *ptr)` does not take a size parameter
- How does the library know the size of the memory region that will be back into free list?
- Most allocators store information in a header block
- Parts of a header
	- The header contains the size of the allocated memory region
	- Additional pointers to speed up de-allocation
	- A magic number for integrity checking
- The size for free region is the size of the header plus the size of the space allocated to the user. 

Embedding a free list
- The memory allocation library initializes the heap and puts the first element of the free list in the free space
- Library cant use `malloc()` to build a list within itself
- Uses memory map to build a heap

---
### Chapter 18 - paging
https://www.youtube.com/watch?v=EukYf1-hAWY&t=127s

Paging
- Instead of allowing segments to have different sizes
- There will be fixed sizes (much smaller)
- Logically separates virtual addresses into pages
- Physical memory is separated into Page frames
- EXACTLY ONE PAGE FOR EVERY PAGE FRAME

Page size is the exact same as the frame size. 
- One thing this instantly solves is free space issues. 
- We don't need to find a space that is "big enough" but instead just goes into a free frame
- OS holds a list of free pages
- Segmentation there were a fixed number of segments 
- On the other hand with pages, we have no clue at compile time how many pages will be needed for the process, its fully dynamic
- The OS keeps track of important tracking data of pages in a page table
- This is a way the OS can map pages into page frames (virtual memory into the actual spots in physical memory)
- Page table has virtual page numbers and they are mapped to physical page frames. THIS IS A PER PROCESS THING (exists in the PCB)
- Very similar to segments. If we have a 64-byte address space then we can use 6 bits to represent the page (2^6 = 64). So just like in segments, the top 2 bits will represent the VPN or virtual page number (used to identify the specific page) and the bottom 4 bits will represent the offset
- 16 byte pages. Note the offset is 16 (2^4) so we know there are 4 pages with size 16B. (16 * 4 = 64)
Address translation
- Hardware puts the VPN into the page table and converts it to the physical frame number. The offset that we found in the original page carries over.![](../../../-images/Pasted%20image%2020251103124727.png)

How is the page table organized?
- This a per process operation
- Page tables can get very very big
- So where do we put it? Can't really put it in the PCB, it is too big
- We throw it all in a table

PTE
- What is used to map a VPN to a physical page number inside of a page table
- It contains the calculated PFN as well as other important bits as listed below. Changes based off ISA

Valid bit
- Keeps track whether a process has requested the data within that page

Protection bits
- Controls the read write etc.

Paging is slow
- Many translations have to happen to actually get the PA or actual data
PTBR - page table base register that tells the OS where the page table begins
- To find PTE address you would add `PTBR + VPN * (Size of PTE)`

--- 
### Chapter 19 Translation lookaside Buffers

TLB
- Part of the MMU (hardware)
- Purpose is to act like a cache to speed up lookup times for specific pages
- Now when translating addresses or trying to access memory, first the OS will route through the TLB and if its a hit it is fast and can just grab data
- If its a miss, then it goes into the page table and then grabs the data. That data is then saved into the TLB to increase hit rate
- Basically it has a chance to skip a step with page lookups greatly speeding up lookup times

Spatial Locality
- Over time you can reduce your amount of misses by grabbing pages that were previously accessed.

Software manages TLB misses
- Raises an exception 
---
### Chapter 20 Paging: Smaller Tables

