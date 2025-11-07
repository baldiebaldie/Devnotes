Operating systems
10-29-2025

### What are condition variables?
- They help threads wait for specific conditions to become true before continuing. 
- For example: A parent thread waits for its child thread before continuing
---
### Two main concurrency problems
1. Mutual Exclusion
	- Making sure only one thread runs at a time
	- This is solved through mutexes
2. Ordering
	- Making sure thread B runs after thread A finishes something
	- This is solved with a condition variable
---
### Key parts of condition variable 
- Queue of waiting threads
	- Threads that are sleeping and waiting
- Wait() 
	- Puts a thread to sleep until a condition is met
- signal()
	- Wakes up a sleeping thread

---
### How wait() works
**When a thread calls wait():**
1. It releases the lock it's holding
2. It goes to sleep
3. When woken up, it gets the lock back before continuing
#### Basic code setup
```c
pthread_cond_t condition;
pthread_mutex_t lock;

//To wait
pthread_cond_wait(&condition, &lock);

//To wake someone up:
pthread_cond_signal(&condition);
```

---

### Simple example: Parent waiting for child to finish

**Problem: Parent creates a child thread and wants to wait until the child finishes**
Bad solution:
```c
//parent
pthread_cond_wait(&cond, &mutex);

//child 
pthread_cond_signal(&cond);
```
Why its broken: If the child finishes before the parent starts waiting, the parent will sleep forever

Good solution:
```c
int done = 0; //Global variable shared between parent and child
```

```c
//parent
void thread_join() {
	pthread_mutex_lock(&mutex);
	while (done = 0)
	{
		pthread_cond_wait(&cond, &mutex);
		pthread_mutex_unlock(&mutex);
	}
}
```

```c
//child
void thread_exit() {
	pthread_mutex_lock(&mutex);
	done = 1;
	pthread_cond_signal(&cond);
	pthread_mutex_unlock(&mutex);
}
```

This ensures that the parent goes to sleep before the child process can obtain the lock, then do its thing, and then wake up the parent to finish.

**NOTE:** When a thread calls wait() and goes to sleep, if it has the lock, **IT GIVES IT UP**.

---

### Producer Consumer Problem
- Think of a restaurant kitchen where producers are the cooks making the food (adding to buffer). You have consumers who are waiters taking food (removing from the buffer), and the buffer is the counter where the food sits.
- Rules:
	- Producers wait when the counter is full (there is too much food, chef doesn't make more)
	- Consumers wait when the counter is empty (No food to take out, so you take a break from being a waiter)
- Example of real code with this issue
```c
int loops; // must initialized somewhere ...
cond_t cond;
mutex_t mutex;

void *producer(void *arg) {
    for (int i = 0; i < loops; i++) {
        Pthread_mutex_lock(&mutex);           // p1
        if (occupied == BSIZE)                // p2
            Pthread_cond_wait(&cond, &mutex); // p3
        put(i);                               // p4
        Pthread_cond_signal(&cond);           // p5
        Pthread_mutex_unlock(&mutex);         // p6
    }
}

void *consumer(void *arg) {
    while (1) {
        Pthread_mutex_lock(&mutex);           // c1
        if (occupied == 0)                    // c2
            Pthread_cond_wait(&cond, &mutex); // c3
        int tmp = get();                      // c4
        Pthread_cond_signal(&cond);           // c5
        Pthread_mutex_unlock(&mutex);         // c6
        printf("%d\n", tmp);
    }
}
  
```

- p1-p3: A producer waits for the buffer to be not full.  
- c1-c3: A consumer waits for the buffer to be not empty.

---

### Three Golden rules for condition variables

Rule 1: Keep State Variables
- Always have a variable that tracks the condition you care about
	- i.e: `int done = 0` or `int buffer_count = 0`
Rule 2: Hold the lock
- Always hold the mutex when calling wait() or signal()
- This prevents race conditions between checking state and waiting
Rule 3: use WHILE not IF
- Always use while loops to check conditions, never if statements
- This is because another thread might change the state between when you're woken up and when you actually run them