Operating systems
10-13-2025

### The problem: Page tables are too big!
- Remember from last chapter: Page tables can get HUGE ad waste tons of memory
**Example calculation:**
- 32-bit address space with 4KB pages
- VPN needs 20 bits (32 - 12 for offset)
- Each PTE is 4 bytes
- Total page table size: 2^20 entries × 4 bytes = **4MB per process**
- For 100 processes: **400MB just for page ta**
**The real problem**: Most of the page table is filled with invalid entries for unused parts of the address space. We're wasting memory storing information about memory we're not even using!

### Simple solution: Bigger pages
Idea: Use larger pages to reduce the number of table entries
**Example with 16KB pages:**
- 32-bit address space with 16KB pages
- VPN needs 18 bits (32 - 14 for offset)
- Page table size: 2^18 entries × 4 bytes = **1MB** (better than 4MB!)
**The problem with this approach**:
- Causes internal fragmentation
- If a process only needs 4KB but gets a 16KB page, it wastes 12KB
- Larger pages mean more wasted space within each page

### Better solution 1: Segmented Page Table (hybrid approach)
**The idea:** Combine segmentation and paging:  (instead of 1 giant page table, have separate smaller pages)
- Instead of one giant page table for the entire address space, have one smaller page table per segment (code, heap, stack)
**How it works**
- Virtual address tells you which segment
- Use that segment's page table to translate
- Only allocate page table entries for pages the segment actually uses
Trade off
- Saves space compared to linear page tables
- wastes space if segments are sparce
- external fragmentation

### Better solution 2: Multi-Level Page Tables (Most importatnt)
- Idea is to turn page table into a tree structure where we only allocate the parts of the address that we actually need.
- **Page Directory** (PD)
	- A table that points to pages of the page table
	- Acts like a table of contents for the page table
- Page directory entry (PDE)
	- Each entry point to one page of page table
	- Has a valid bit: If 0, that chunk does not exist (saves space)
-  Page Directory Base Register (PDBR):
	- Hardware register which points to the page directly
	- This replaces the Page table base register (PTBR)
- Levels
	- How many levels of indirection
	- level-2 =  page directory -> page table -> physical page
	- level-3 = top directory -> page directory -> page table -> physical page
### How multi level works
Its like a table of contents in a book
1. Page directory (Table of contents)
	- List of pages
	- if valid bit 0, nothing is there
2. Pages of Page table (Individual chapters)
	- Only exists if contains valid entry
3. Physical page table (contents of the book)

**Virtual Address Breakdown:**
```
| Page Directory Index | Page Table Index | Offset |
```


### Translation steps:
1. Use page directory index to find which page table page to use
2. Use page table index to find the PTE within the page
3. Use offset to find position within the physical page

**Formula: Finding a PTE**
`PTE Address = (PDE.PFN << SHIFT) + (PTIndex × sizeof(PTE))`

- ---
- ---
- ---
- ---

### Understanding multi-level paging

**The core concept:**
What is the problem we are solving?
- Imaging our computer has a huge notebook (virtual memory) where programs think they can write anywhere. But the actual desk space (Physical memory/RAM) is much smaller. We need a system to translate notebook page numbers into actual desk locations
- The page table is the phonebook that translates virtual addresses to physical addresses
Why not use one big table?
- Uses too much memory
**The solution: Multi-Level paging**
- Instead of one big table, we break it into multiple smaller tables arranged in levels, like a tree
- **Level 1** (Page Directory): Points to smaller tables
- **Level 2** (Page Table): Points to actual physical frames
- (Sometimes Level 3, 4, etc. for larger address spaces)
This way, we only create small tables we actually need.

### How the address gets split up
A virtual address gets divided into parts
- Level 1 index: Which entry in the top-level table
- Level 2 index: Which entry in the second-level table
- Offset: Which byte within the final page

Virtual representation of what happens

```
Program wants: Virtual Address 5000
                      |
         [Break into parts: 01|0110111|00101000]
                      |
                      v
    ┌─────────────────────────────────┐
    │   Level 1: Page Directory       │
    │   (Lives in physical memory)    │
    │                                 │
    │   Entry 0: → Points to Table A  │
    │ → Entry 1: → Points to Table B  │ ← We look here (index 01)
    │   Entry 2: → Points to Table C  │
    └─────────────────────────────────┘
                      |
          "Go to Table B at address 0x8000"
                      |
                      v
    ┌─────────────────────────────────┐
    │   Level 2: Page Table B         │
    │   (Lives in physical memory)    │
    │                                 │
    │   Entry 0: → Frame 10           │
    │   ...                           │
    │ → Entry 55: → Frame 23          │ ← We look here (index 0110111)
    │   ...                           │
    └─────────────────────────────────┘
                      |
          "Data is in Physical Frame 23"
                      |
                      v
    ┌─────────────────────────────────┐
    │   Physical Frame 23             │
    │                                 │
    │   [offset 00101000]             │
    │   "hello world" ← ACTUAL DATA   │ ← Data lives HERE
    │                                 │
    └─────────────────────────────────┘
```
---
---
---
## Reading the textbook
-  Page tables are too bag and use too much memory
- Remember 1 page table entry for each process in system

#### Crux how do we make page tables smaller

### Solution 1: Bigger pages
-  We can make size of pages bigger which makes page table smaller
- Issue: There is a lot of wasted space within the actual pages themselves

### Hybrid approach: Paging and segments
- Instead of having a single page table entry for the entire address space of the process, why not have one per logical segment? 
- Will have base register, bound or limit register just like in segmentation
- base: where each segment lived in physical memory 
- bound or limit: size of the segment in the MMU
- Here we use the base not to point to the segment but hold the physical address of the page table segment
Issues with this approach
- There is still potential for a lot of page table waste.
- External fragmentation can arise because page tables can be arbitrary in size

### Multi-level page tables
- Turn the linear page table into a tree
- Basically a filing system with multiple drawers and folders instead of having one huge table.
	- Chop up the page table into page sized units
	- If an entire page of page-table entries is invalid, don't allocate the page of the page table at all
	- To track whether a page of the page table is valid, use a new structure called page directory4
- Why do we need them?
	- Imaging 32 bit computer. this means we can have 2^32 different addresses. If each page is 4KB (2^32/2^12) there are 2^20(1GB) different page table entries. With multilevel page tables we only need to create the parts we actually need. This saves a ton of memory
**EXAMPLE**
- 20 bit virtual address and 1KB pages
- would be split up like this
 ![](../../images/Pasted%20image%2020251020131415.png)
1. Look at the first level index. This gives the number from 0 - 1023 which can be used to find the right second level page table.
2. The offset will say which entry exactly in the second level page table to use
3. That entry gives us the physical page number


Virtual address: 0x52400 (which is 001010010010000000000 in binary)
- first 10 bits 0010100100 = 164
- second 10 bits 10000000000 = 512
- So this says go to entry 164 in the first level page table which points to the second page table
- in the second level page table go to entry 512 which gives us a physical page number (lets say 89)
- So the final physical address is 89 * 1024 + 512 = 91648

The benefit of multi level page tables is that they save memory because empty sections do not need to exist. If a program only uses small parts of its virtual address space, most of the page tables never get created. The trade off is that translation takes more steps, but the TLB cache helps make this fast in practice. 

Think of it like:
- **First level = array of pointers** Each entry in the first level page table is just a reference (pointer) to a second level page table. It doesn't contain the actual translation data.
- **Second level = actual PTEs with data** Each entry in the second level page tables contains the real page table entry (PTE) data. This includes the physical page number, permission bits, valid bit, and other flags.
- Once you find the physical page number you combine that with the offset to get the physical address.
- `Physical address = (PPN * Page Size) + Offset`

---
