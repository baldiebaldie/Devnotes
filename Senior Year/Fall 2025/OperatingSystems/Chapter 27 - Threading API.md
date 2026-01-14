Operating systems
10-20-2025

### Thread
>A process is a running program. Each process needs at least one thread
>A thread is a single sequence of instructions within a process.
>Threads are considered lightweight processes
>If a process is a kitchen then a thread is a single chef

---
### Why use threads?
- using multiple threads is called parallelism or concurrency.
- It lets a program do many things at once
- This makes the program faster and more responsive. 

A new abstraction for a single running process
- A multi-threaded program has more than one point of execution
- Multiple PCs (Program counter)
- multiple threads of the same process share the same address space
Each thread has its own PC so it knows where it is

You can create threads through:
- `pthread_create()`
	- This function needs 4 things
		1. Thread ID
		2. Attributes (usually just pass NULL for defaults)
		3. The function the thread should run
		4. Arguments to pass to that function
- `pthread_join()`
	- This makes the main program wait until a specific thread finishes its work
		i.e.
	- `pthread_join(worker, &result;`
	- the main thread will pause here until the worker thread is done
### Important memory rule
- Never return a pointer to something created on the thread's stack. When the thread ends the memory gets destroyed.
---
### Locks (mutual exclusion)
- Locks prevent multiple threads from messing with the same data at once. 
- `pthread_mutex_lock()` - lock the door
- `pthread_mutex_unlock()` - unlock the door
- This only lets one thread into the "Critical space" and thus that is the only thread that can update the shared data

For example
``` c
pthread_mutex_t bathroom_lock;
pthread_mutex_lock(&bathroom_lock);
//use the shared resource (bathroom)
count = count + 1;
pthread_mutex_unlock(&bathroom_lock);
```

---
### Lock initialization
- Locks must be set up before use. You can do this in two ways
1. (STATIC WAY)`pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER`
2. (DYNAMIC WAY) 
   ```c 
   pthread_mutex_t lock;
   pthread_mutex_init(&lock, NULL);
   ```

You always want to check if these functions succeed. Failing silently can let multiple threads into critical sections at once which is a no no.

---

### Condition variables
There are used when one thread needs to wait for another thread to do something specific.
- `pthread_cond_wait()` - this waits for the signal
- `pthread_cond_signal()` -  sends the signal
Our analogy is us waiting on the signal that dinner is ready
Example - waiting:
```c
pthread_mutex_lock(&lock);
while (dinner_ready == 0) 
{
	pthread_cond_wait(&dinner_signal, &lock);
}
pthread_mutex_unlock(&lock)
```
Example - Signaling:
```c 
pthread_mutex_lock(&lock);
dinner_ready = 1;
pthread_cond_signal(&dinner_signal);
pthread_mutex_unlock(&lock);
```

---
#### The key rules
1. Always use locks with condition variables - you need both the lock and the condition variable to work together safely
2. Use while loops, not if statements - always recheck the condition after waking up. Sometimes the thread wakes up for no reason
3. Do not use simple flags - Using a variable like `while (ready -- 0`); wastes CPU and is error-prone
4. Check return codes - These functions can fail. Always check if they succeeded.

---

### Higher level intuition
Think of multi-threading like managing a kitchen
- Multiple cooks (threads) work on different parts of the meal
- They share equipment (memory) but each has their own workspace (stack)
- Some equipment needs to be used by one cook at each time. A order is urgent and needs to be done first. (locks)
- Cooks need to coordinate timing (condition variables)
- The head chef needs to know when each dish is done (`pthread join`)
The main challenge is not learning the API - its designing programs so threads work together without stepping on each others toes or creating deadlocks

---
### Context switching between threads
Each thread has a context which includes
- CPU registers
- Program counter
- Stack pointer
Thread context switching has a lower overhead cost
1. Save the threads current state
2. Load the next threads state
3. Jump to the new thread

The main difference is that process context switching is slower. Almost like process context switch is moving through buildings while thread context switching is like moving through rooms

---
