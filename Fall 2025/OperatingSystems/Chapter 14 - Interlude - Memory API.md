Operating systems
09-22-2025

### Why do we need Dynamic Memory?
- We need dynamic memory allocation because we do not always know how much memory we will need when a program is written.
- The main reasons programs need dynamic memory:
	- Unknown size at compile time: We do not know how much data the user will input
	- Recursive functions: Functions that call itself might need different amounts of memory each time
	- Complex data structures: Things like linked list and trees grow and shrink
---
### Two types of dynamic memory: Stack Vs Heap
#### Stack memory
The stack is like a stack of plates, you can only add or remove plates from the top
- When a function is called, its variables get pushed onto the stack
- When the function ends, its variables get popped off the stack
- LIFO (last in first out): The thing added last is the first thing removed.
- **No fragmentation**: Memory is always organized and clean
- Fast access to memory
i.e. 
``` c
void func() { int x; // This goes on the stack }
``` 
#### Heap memory
The heap is like the big messy warehouse where you can store things anywhere
- You must explicitly ask for memory using malloc()
- You must explicitly give it back using free()
- Unpredictable order: You can allocate and free in any order
- Fragmentation: Can have scattered free spaces
- Indirect access: You need a pointer to find your data
i.e.
```c
void func() {
    int *x = malloc(sizeof(int)); // This goes on the heap
}
```

#### Understanding memory segments
When you write a program, different things go in different places
- Code segment: Where your actual program instructions live
- Static data segment: Global variables that exist for your entire program
- Stack segment: Local variables and functions call information
- Heap segment: Dynamically allocated memory

##### The malloc() function
`malloc()` is like asking the computer "Can i borrow some space?"
**Parameters:**
- `size`: How many bytes you want (use `sizeof()` to get the right amount)
**Returns:**
- **Success:** A pointer to your new memory space
- **Failure:** NULL (means "sorry, no memory available")
i.e.
```c
double *d = (double *) malloc(sizeof(double));
```

This asks for enough space to hold one double value. `double *`
The operator sizeof() tells you how big something is in bytes and is commonly used in malloc functions.
#### Memory allocation example
1. Request memory
- `ptr = malloc(5 * sizeof(int))`
2. Use the memory: You can now store 5 integers
3. Free the memory `free(ptr); ptr = NULL` 
- This returns memory to the system
- sets pointer to NULL for safety
---
#### What are brk() and sbrk() statements?
- brk() and sbrk() are system calls that manage the "program break" - think of this as a fence that marks where your program's heap memory ends.
- Program break: The boundary line between your program can use and memory it cannot use
- brk() moves the fence to specific locations you choose.
- sbrk() moves the fence forward or backwards a certain distance.
- sbrk() returns the previous break value.
#### Example:
![](../../images/Pasted%20image%2020250922145859.png)
##### Why Second Execution Shows Big Jump
**Expected:** Small 5-byte increments **Actual:** Jump from `0x5622cd695000` to `0x5622cd6b6000` (much larger)
##### What Happened
Between Line A and Line B, `printf()` allocated a large internal buffer:
1. **Line A:** `sbrk(0)` returns current break position
2. **Hidden step:** `printf()` from Line A needs to print, so it calls `malloc()` for buffer space
3. **Hidden step:** `malloc()` calls `sbrk()` internally, moving break forward ~1000+ bytes
4. **Line B:** Our `sbrk(5)` returns the old position (before our 5-byte move)
##### Key Points
- `printf()` is lazy - doesn't allocate buffer until it needs to print
- Our expected 10-byte pattern is still there, just hidden by printf's large allocation
- Different executions show different timing of when printf allocates its buffer
- `sbrk()` always returns the OLD position before moving
##### Takeaway
The large jump isn't from our code - it's from printf's internal buffer allocation happening between our calls.
##### Top-Down Data Flow (OS Perspective)

```
1. USER PROGRAM
   ↓
   "I need more memory" → calls malloc(1000)

2. MALLOC LIBRARY
   ↓
   • Checks internal free list for 1000 bytes
   • If not enough free space available
   ↓
   "Need more heap space" → calls sbrk(4096)

3. SYSTEM CALL INTERFACE
   ↓
   • User space → Kernel space transition
   • Validates the request
   ↓

4. KERNEL MEMORY MANAGER
   ↓
   • Checks if process can have more memory
   • Verifies memory limits not exceeded
   • Checks virtual memory availability
   ↓

5. VIRTUAL MEMORY SYSTEM
   ↓
   • Updates process memory map
   • Allocates virtual address space
   • Updates page tables
   ↓

6. PHYSICAL MEMORY MANAGER
   ↓
   • May allocate physical pages (lazy allocation)
   • Updates memory tracking structures
   ↓

7. RETURN PATH
   ↓
   Kernel → System Call Interface → malloc() → User Program
   "Here's your memory address"
```

## Step-by-Step Process

1. **Your program** calls `malloc(1000)`
2. **malloc library** looks for free space in its internal lists
3. **If not enough space:** malloc calls `sbrk(4096)` to get more heap space
4. **Kernel** receives the sbrk request
5. **Kernel** checks if the process is allowed more memory
6. **Kernel** updates the program break pointer
7. **Kernel** marks new virtual memory pages as available
8. **Return success** back through all layers
9. **malloc** carves out 1000 bytes from the new space
10. **Your program** gets a pointer to use

---
### Common Memory Errors
#### 1. **Forgetting to allocate memory**
i.e.
```c 
char *dst; // Not allocated!
strcpy(dst, "hello"); // CRASH! Segmentation fault
```
The fix is:
```c 
char *dst = malloc(strlen("hello") + 1); // +1 for '\0'
strcpy(dst, "hello"); // Now it works
```
#### 2. **Not Allocating enough memory**
i.e.
```c
char *dst = malloc(strlen("hello")); // Missing +1 for '\0'
strcpy(dst, "hello"); // Writes beyond allocated space
```
The string "hello" needs 6 bytes (h-e-l-l-o-\0), but you only allocated 5.
The fix:
```c 
char *dst = malloc(strlen("hello") + 1); // +1 for '\0'
strcpy(dst, "hello"); // Now it works
```
#### 3. **Forgetting to initialize**
```c
int *x = malloc(sizeof(int));
printf("%d", *x); // Prints random garbage value
```
The fix is:
```c
int *x = malloc(sizeof(int));
*x = 0; // Initialize to 0
```
#### 4. **Memory leaks**
This happens when you allocate memory but never free it. 
```c
int *a = malloc(sizeof(int)); // Allocate
int *b = malloc(sizeof(int)); // Allocate more
// Program ends without calling free() - MEMORY LEAK!
```
**Fix:** Always free what you allocate:
```c
free(a);
free(b);
```
#### 5. **Dangling Pointer**
This happens when you free memory but keep using the pointer. 
```c
int *x = malloc(sizeof(int));
free(x); // Memory is freed
*x = 5; // DANGER! Using freed memory
``` 
#### 6. **Double free**
This happens when you free the same memory twice.
```c
int *x = malloc(sizeof(int));
free(x);
free(x); // ERROR! Already freed
```
**Fix:** Only free once, and set to NULL:
```c
free(x);
x = NULL;
```
---
### Other Memory Functions

#### calloc() - Clean Allocation

`calloc()` is like `malloc()` but it also cleans the memory (sets it to zero).
```c
int *ptr = calloc(5, sizeof(int)); // Allocates and zeros out
```

This creates 5 integers, all set to 0.
#### realloc() - Resize Memory

`realloc()` lets you change the size of already allocated memory.

```c
int *ptr = malloc(5 * sizeof(int)); // 5 integers
ptr = realloc(ptr, 10 * sizeof(int)); // Now 10 integers
```

**Important:** The pointer might change, so always assign the result back to your pointer.
