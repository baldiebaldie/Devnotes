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

---

A sloppy counter is a technique to make counting faster when you have multiple threads, but you're okay with the count being approximate instead of perfectly accurate.

## The Problem with Regular Counters

**Accurate counter (slow):**

```
global_counter = 0;
lock = 0;

void increment() {
    acquire_lock();
    global_counter++;  // only one thread can do this at a time
    release_lock();
}
```

This is slow because every single increment has to wait for the lock. If you have 8 threads all trying to count, they form a line and wait.

## Sloppy Counter Solution

**Give each thread its own private counter:**

```
global_counter = 0;
local_counter[thread_0] = 0;
local_counter[thread_1] = 0;
local_counter[thread_2] = 0;
// ... one for each thread
```

## How It Works

**Most of the time (fast path):**

```
void increment() {
    local_counter[my_thread_id]++;  // no lock needed!
}
```

Each thread just increments its own private counter. Super fast, no waiting.

**Occasionally (slow path):**

```
void increment() {
    local_counter[my_thread_id]++;
    
    if (local_counter[my_thread_id] >= THRESHOLD) {
        acquire_lock();
        global_counter += local_counter[my_thread_id];
        local_counter[my_thread_id] = 0;  // reset local counter
        release_lock();
    }
}
```

When a thread's local counter gets "big enough" (like 100), it takes the lock and dumps all its counts into the global counter.

## Example with Threshold = 5

```
Time 1: Thread A increments → local_A = 1, global = 0
Time 2: Thread B increments → local_B = 1, global = 0  
Time 3: Thread A increments → local_A = 2, global = 0
Time 4: Thread A increments → local_A = 3, global = 0
Time 5: Thread A increments → local_A = 4, global = 0
Time 6: Thread A increments → local_A = 5, hits threshold!
        Thread A: global = global + 5 = 5, local_A = 0
Time 7: Thread B increments → local_B = 2, global = 5
```

## Getting the Total Count

```
int get_total() {
    acquire_lock();
    total = global_counter;
    for (each thread) {
        total += local_counter[thread];
    }
    release_lock();
    return total;
}
```

Add up the global counter plus all the local counters.

## Why It's "Sloppy"

The count is approximate because:

- Local counters haven't been "flushed" to global yet
- At any moment, the real total might be different from what you see
- If you ask "what's the count?" you get close, but not exact

## Why This Is Useful

**Performance:** Most increments are super fast (no locks). Only every 100th increment (or whatever threshold) needs to take a lock.

**Good enough:** For many applications, you don't need the exact count every millisecond. You just need "approximately how many times has this happened?"

**Examples where sloppy counters work:**

- Web server hit counters
- Memory allocation statistics
- Performance monitoring
- Cache miss counts

**Examples where you need exact counts:**

- Bank account balances
- Inventory management
- Critical system resources

The key trade-off is speed vs accuracy. Sloppy counters give you most of the speed benefits with "good enough" accuracy for many real-world uses.


---

# Concurrent linked lists

A concurrent linked list is a linked list that multiple threads can use at the same time safely. Regular linked lists break when multiple threads try to modify them simultaneously.

## The Problem with Regular Linked Lists

**Normal linked list (breaks with multiple threads):**

```
struct Node {
    int data;
    Node* next;
};

Node* head = NULL;

void insert(int value) {
    Node* new_node = malloc(sizeof(Node));
    new_node->data = value;
    new_node->next = head;  // Step 1
    head = new_node;        // Step 2
}
```

**Race condition example:**

```
Thread A wants to insert 5:
1. Creates new node with data=5
2. Sets new_node->next = head (points to old first node)
3. About to set head = new_node...

Thread B wants to insert 10:
4. Creates new node with data=10  
5. Sets new_node->next = head (still points to old first node!)
6. Sets head = new_node (now head points to 10)

Thread A continues:
7. Sets head = new_node (now head points to 5)

Result: We lost the node with 10! It's not in the list anymore.
```

## Solution 1: Coarse-Grained Locking

**Protect the entire list with one big lock:**

```
struct ConcurrentList {
    Node* head;
    Lock list_lock;
};

void insert(ConcurrentList* list, int value) {
    acquire_lock(&list_lock);
    
    Node* new_node = malloc(sizeof(Node));
    new_node->data = value;
    new_node->next = list->head;
    list->head = new_node;
    
    release_lock(&list_lock);
}

bool search(ConcurrentList* list, int value) {
    acquire_lock(&list_lock);
    
    Node* current = list->head;
    while (current != NULL) {
        if (current->data == value) {
            release_lock(&list_lock);
            return true;
        }
        current = current->next;
    }
    
    release_lock(&list_lock);
    return false;
}
```

**Pros:** Simple and safe **Cons:** Only one thread can use the list at a time, even for reading

## Solution 2: Fine-Grained Locking

**Give each node its own lock:**

```
struct Node {
    int data;
    Node* next;
    Lock node_lock;
};

void insert(ConcurrentList* list, int value) {
    Node* new_node = malloc(sizeof(Node));
    new_node->data = value;
    
    acquire_lock(&list->head_lock);
    new_node->next = list->head;
    list->head = new_node;
    release_lock(&list->head_lock);
}

bool search(ConcurrentList* list, int value) {
    acquire_lock(&list->head_lock);
    Node* current = list->head;
    if (current != NULL) {
        acquire_lock(&current->node_lock);
    }
    release_lock(&list->head_lock);
    
    while (current != NULL) {
        if (current->data == value) {
            release_lock(&current->node_lock);
            return true;
        }
        
        Node* next = current->next;
        if (next != NULL) {
            acquire_lock(&next->node_lock);
        }
        release_lock(&current->node_lock);
        current = next;
    }
    
    return false;
}
```

**The "hand-over-hand" technique:** Hold the current node's lock, grab the next node's lock, then release the current lock. Like climbing a rope.

**Pros:** Multiple threads can traverse different parts of the list **Cons:** Lots of lock overhead, complex to get right

## Solution 3: Lock-Free (Advanced)

**Use atomic operations instead of locks:**

```
void insert(ConcurrentList* list, int value) {
    Node* new_node = malloc(sizeof(Node));
    new_node->data = value;
    
    do {
        new_node->next = list->head;
    } while (!compare_and_swap(&list->head, new_node->next, new_node));
}
```

**How this works:**

1. Set new_node->next to current head
2. Try to atomically change head from old value to new_node
3. If someone else changed head, loop and try again
4. Keep trying until we succeed

**Pros:** No locks, very fast **Cons:** Much harder to implement correctly, especially for delete operations

## Why Concurrent Lists Are Tricky

**The ABA problem:**

```
Thread A: Reads head = Node1
Thread B: Removes Node1, then adds it back
Thread A: Sees head = Node1, thinks nothing changed, but the list structure might be different!
```

**Memory management:** When can you safely free a deleted node? Other threads might still be reading it.

**Ordering guarantees:** What happens if thread A inserts 5 and thread B inserts 10 at the same time? Which order do they appear in?

## Real-World Usage

Most programs use:

- **Libraries** (like Java's ConcurrentLinkedQueue) instead of writing their own
- **Read-mostly data structures** where you can use techniques like RCU (Read-Copy-Update)
- **Simpler alternatives** like concurrent arrays or hash tables when possible

The key insight is that making data structures thread-safe is much harder than it looks, which is why most developers rely on well-tested library implementations rather than rolling their own.
