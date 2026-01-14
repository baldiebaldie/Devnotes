Operating systems
11-10-2025

### What are concurrency bugs?
- Happen when multiple threads run at the same time and interfere with each other.
2 main types
1. Non-Deadlock bugs
2. Deadlock bugs

---
### Non-deadlock bugs
Atomicity Violation
- What it is: a section of code should run completely without interruption but it gets interrupted
- For example:
	- Thread 1 checks if data exists, then thread 2 deletes the data, then thread 1 tries to use the data and it crashes
- How to fix it: Use locks to protect the code section so only one thread can access it at a time
#### Order violation
- What it is: One action should happen before another, but they happen in the wrong order
- For example:
	- Thread 2 tries to use something that thread 1 hasn't finished creating yet
- How to fix it: Use condition variables or semaphores to make sure things happen in the right order

---
### Deadlock bugs

#### What is a deadlock?
- Deadlock happens when two or more threads are stuck waiting for each other. Neither can continue because each is waiting for the other to release something
- For example:
	- Thread 1 holds lock A and wants lock B
	- Thread 2 holds lock B and wants lock A
	- Both threads are stuck waiting forever
#### Four conditions for deadlock
All four of these must be true for a deadlock to happen
1. Mutual exclusion: Only one thread can use a resource at a time
2. Hold-And-Wait: Threads hold resources while waiting for more resources
3. No Preemption: Resources cannot be taken away from threads by force
4. Circular wait: Threads wait for resources in a circular chain

----
### Ways to handle deadlock 
1. Deadlock Prevention
	- Stop deadlock by eliminating one of the four conditions:
	Prevent Circular wait:
	- Always acquire locks in the same order
	- Use lock address to determine order
	Prevent Hold-and-Wait:
	- Acquire all locks at once
	- Problem: Reduces performance because threads must get all locks early
	Prevent no preemption:
	- Use trylock() - if you can't get a lock, release what you have and try again
	- Problem: Can cause live lock (threads keep trying and failing repeatedly)
	- Solution: Add random delays between attempts
	Prevent Mutual Exclusion:
	- Use lock-free programming with atomic operations
	- Replaces locks with compare-and-swap instructions
2. Deadlock Avoidance 
	- Requires knowing what locks each thread will need ahead of time
	- Schedule threads carefully so deadlock cannot happen
	- Problem: Reduces performance and requires advance planning
3. Deadlock Detection and Recovery
	- Let deadlock happen, then detect and fix it
	- Build a graph of which threads are waiting for which resources
	- Look for cycles in the graph
	- When deadlock is found, restart the system or kill some threads
	- Used by many database system
	