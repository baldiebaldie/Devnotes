Operating systems
10-27-2025

### Sloppy counter
A technique to make countering faster when you have multiple threads
- The problem with regular counters is they are very slow. Every increment has to wait for the lock. With 8 threads all trying to count, it will take a while.
Sloppy counter gives each thread its own private counter while also keeping track of a global counter

### Concurrent linked list
- Normal linked lists breaks with multiple threads due to several race conditions
- One example is multiple threads wanting to add nodes (lets say 5 and 10) but due to how adding nodes in a linked list works, if one thread points head to the new_node next then everything messes up for the other thread.
### Solution 1: Coarse-Grained locking
- Protect the entire list with one big lock
### Solution 2: Fine-Gained Locking

Give each node its own lock