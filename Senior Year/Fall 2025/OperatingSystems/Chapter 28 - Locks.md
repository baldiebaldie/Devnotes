Operating systems
10-20-2025


---
###  ![NOTES FROM THIS VIDEO](https://www.youtube.com/watch?v=clkO9nzwopU)

LOCKS:
	How are locks implemented?
	`A lock is a mechanism that we can use to control access of threads to critical sections`
We want to make high level statements atomic for threads
- Locks are basically variables
- Locks are used to allow programmers to regain some control over the scheduler and allow for more say in the execution order of some threads
`mutex = mutual exclusion`

**Control interrupt**
- What if we allow threads to enable and disable interrupts. This allows single threads to be atomic
	- The issue is it takes control away from the OS which is really bad
- What if we use flags?
	- It does not actually work because if there is an interrupt before a thread can say it has a lock then there is a chance that another thread can see try and take the lock even though it is already in use causing multiple threads to hold a lock at once defeating the entire purpose of the lock
- Test and set
	- A single atomic assembly language instruction
	-  Fetches old value, store new value, return old value
	- This forces mutual exclusion between le threads because the instruction itself is atomic 
	- This approach is not very fair or very efficient
- Compare and swap
	- Atomic operation that takes a value from memory and compares it to what we expect that value to be. If it is the value we expected then update the value in memory to new. Always returns the original value. 
- Load-Linked
	- Pair of instructions that work in concert to help build critical sections. 
- Fetch and add
	- Literally grabs from memory and adds one
	- Can be used in a ticket system
	- Basically the thread that comes first will have a turn and then when its done, will unlock and set turn to 1 for the next thread.
	- This basically creates a queue for threads 
- Does spinning suck?
	- Instead of spinning if you know you need to spin, just give up your time slice to scheduler so the thread does not sit doing nothing
		- Sucks because it usually will go back and forth between one thread finishing its times slice in the lock and the other thread spinning
		- Spends way too much time context switching rather than wasting time spinning
	- Instead of giving up to the processor you can create a queue and allow threads to put itself to sleep to prevent the OS context switching to it while its spinning


### Specifics
Test and set
- Threads will check if a critical section has the flag 0 or 1
- 0 means its open and thread can go in
- 1 means there is already a thread inside
	- So spin
	- Once open go inside critical section and do stuff
``` c
while (test_and_set(flag) == 1) {
    // wait here, someone else is using the resource
}
// now I'm inside the critical section
do_important_work();
flag = 0; // release the lock
```
Compare and swap
- We expect the lock to be in use so we wait
	- when open, swap flag to 1
``` c
while (compare_and_swap(lock, 0, 1) != 0) {
    // wait here, the lock wasn't 0 like we expected
}
// now we have the lock (we changed 0 to 1)
do_important_work();
lock = 0; // release
```

Load Linked
- Like a 2 step processes where you reverse something and then try and claim it
	1. Load linked reads a memory location and marks it as watched
	2. Do whatever you need to do
	3. Store conditional tries to write back but only succeeds if nobody else touched that memory location since your load-linked
	4. If someone changed it, you store fails and you start over
``` c
do {
    count = load_linked(counter);
    new_count = count + some_complex_calculation();
} while (!store_conditional(counter, new_count));
```

1. Load-linked reads the lock value (let's say it's 0) and tells the hardware "watch this spot" 
2. We check if it's free (0 means free) 
3. Store-conditional tries to write 1 to claim the lock 
4. If the hardware says "nobody else touched this spot," the store succeeds 
5. If someone else changed it, the store fails and we loop back to try again

#### Why use this instead of check and swap 
It is more flexible
- You can do complex calculations between the load and store
- Test and set and compare and swap force you to decide everything 
It is better for building advanced data structure:
- You can read multiple values, think about them, then update



### Lock implementation: Block when waiting
- You can use `park()` for blocking 
- we can use guard as a way to force threads to go to sleep rather than looping through a while loop waiting for a lock. 
- Waiting for a lock is very inefficient and a waste of a time slice.
- Using a guard, the thread can then check to see if the lock is available and then if not, then add it to the queue and park(). 
### Extra Claude insight on lock structure with blocking

## The Basic Idea

Instead of threads constantly checking "is the lock free yet?" (spinning), we put them to sleep and wake them up when it's their turn. This saves CPU power.

## The Lock Structure

```c
struct Lock {
    int guard = 0;        // protects the lock state
    int locked = 0;       // is someone using the critical section?
    queue waiting_threads; // list of sleeping threads
}
```

## Lock Operation (Acquiring the Lock)

```c
void lock() {
    while (test_and_set(guard) == 1) {
        // spin briefly to get the guard
    }
    
    if (locked == 0) {
        // critical section is free!
        locked = 1;
        guard = 0;  // release guard
        return;     // we got the lock
    }
    
    // someone else is using it, so we need to sleep
    add_myself_to_queue(waiting_threads);
    guard = 0;  // MUST release guard before sleeping
    park();     // go to sleep, OS will wake us later
}
```

**Step by step:**

1. **Get the guard:** Use test-and-set to get exclusive access to the lock's internal state
2. **Check if free:** Look at the `locked` flag
3. **If free:** Set `locked = 1`, release guard, and we're done
4. **If busy:** Add ourselves to the waiting queue, release the guard, then go to sleep

**Key point:** We MUST release the guard before calling park(). Otherwise, the thread holding the lock can't release it!

## Unlock Operation (Releasing the Lock)

```c
void unlock() {
    while (test_and_set(guard) == 1) {
        // spin to get the guard
    }
    
    if (queue_is_empty(waiting_threads)) {
        // nobody waiting, just mark as free
        locked = 0;
        guard = 0;
        return;
    }
    
    // someone is waiting, wake them up
    thread = remove_from_queue(waiting_threads);
    guard = 0;  // release guard
    unpark(thread);  // wake up the waiting thread
}
```

**Step by step:**

1. **Get the guard:** Protect the lock state while we modify it
2. **Check waiting queue:** Are any threads sleeping and waiting?
3. **If nobody waiting:** Just set `locked = 0` and we're done
4. **If someone waiting:** Remove a thread from the queue and wake it up with unpark()

**Key point:** We don't set `locked = 0` when waking someone up. The woken thread will set `locked = 1` when it runs.

## Why This Works Better

**Spinning version (bad):**

```c
while (test_and_set(lock) == 1) {
    // waste CPU cycles checking over and over
}
```

**Blocking version (good):**

- Waiting threads sleep and use zero CPU
- OS scheduler can run other useful work
- Much more efficient with many threads

## The Tricky Part

There's a race condition we have to avoid:

```c
// WRONG WAY - race condition!
if (locked == 1) {
    guard = 0;           // release guard
    park();              // what if unlock() happens here?
}
```

If unlock() runs between releasing the guard and calling park(), we might sleep forever because the wakeup signal gets lost.

**The fix:** Add ourselves to the queue BEFORE releasing the guard. This guarantees unlock() will see us and wake us up.

## Complete Flow Example

**Thread A wants the lock:**

1. Gets guard, sees lock is free, sets locked=1, releases guard ✓

**Thread B wants the lock:**

1. Gets guard, sees locked=1, adds self to queue, releases guard, sleeps 😴

**Thread C wants the lock:**

1. Gets guard, sees locked=1, adds self to queue, releases guard, sleeps 😴

**Thread A finishes:**

1. Gets guard, sees queue has threads B and C, wakes up thread B, releases guard
2. Thread B wakes up, already has the lock (locked stays 1)

This way only one thread runs at a time, and the others sleep peacefully instead of wasting CPU.


---


Yes! There are actually several potential race conditions in the lock structure I showed. Let me walk through the main ones:

## Race Condition 1: The Wakeup Miss

This is the big one I mentioned briefly:

```
Thread A (trying to lock):
1. Gets guard
2. Sees locked = 1
3. Adds self to queue
4. Releases guard
5. About to call park()...

Thread B (unlocking):
6. Gets guard  
7. Sees queue has Thread A
8. Releases guard
9. Calls unpark(Thread A)  ← wakeup signal sent!

Thread A:
10. Calls park()  ← goes to sleep AFTER the wakeup!
```

**Result:** Thread A sleeps forever because it missed its wakeup signal.

**The fix:** The OS typically handles this by making unpark() "sticky" - if you unpark a thread that isn't sleeping yet, the next park() call returns immediately.

## Race Condition 2: Guard Release Timing

```
Thread A (unlocking):
1. Gets guard
2. Removes Thread B from queue
3. Releases guard  ← releases too early!
4. Calls unpark(Thread B)

Thread C (trying to lock):
5. Gets guard (while Thread B is waking up)
6. Sees empty queue, sets locked = 0  ← wrong!
7. Takes the lock

Thread B:
8. Wakes up, assumes it has the lock
```

**Result:** Both Thread B and Thread C think they have the lock!

## Race Condition 3: Queue Corruption

```
Thread A: Gets guard, about to add self to queue...
Thread B: Gets guard somehow, also adds self to queue
```

This shouldn't happen if test-and-set works correctly, but shows why the guard is critical.

## Better Implementation

Here's a more robust version:

```
void lock() {
    while (test_and_set(guard) == 1);  // get guard
    
    if (locked == 0) {
        locked = 1;
        guard = 0;
        return;
    }
    
    add_myself_to_queue(waiting_threads);
    // Key: DON'T release guard here yet
    setpark();  // tell OS we're about to sleep
    guard = 0;  // NOW release guard
    park();     // sleep (but setpark protects us)
}

void unlock() {
    while (test_and_set(guard) == 1);  // get guard
    
    if (queue_is_empty(waiting_threads)) {
        locked = 0;
    } else {
        thread = remove_from_queue(waiting_threads);
        unpark(thread);  // don't change locked - waking thread owns it
    }
    
    guard = 0;
}
```

**Key insight:** The fundamental issue is that we need to atomically "add to queue AND start sleeping" but we can't hold the guard while sleeping (deadlock). Different OS designs solve this in different ways - some use the setpark/park pattern, others use futexes, etc.

The race conditions are why real-world locking is quite complex and why most people use library implementations rather than rolling their own!
