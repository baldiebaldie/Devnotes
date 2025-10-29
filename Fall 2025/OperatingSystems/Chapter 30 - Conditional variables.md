Operating systems
10-29-2025

### What are condition variables?
- They help threads wait for specific conditions to become true before continuing. 
- For example: A parent thread waits for its child thread before continuing
### 2 main concurrency problems
1. Mutual Exclusion
	- Making sure only one thread runs at a time
	- This is solved through mutexes
2. Ordering
	- Making sure thread B runs after thread A finishes something
	- This is solved with a condition variable
### Key parts of condition variable 
- Queue of waiting threads
	- Threads that are sleeping and waiting
- Wait() 
	- Puts a thread to sleep until a condition is met
- signal()
	- Wakes up a sleeping thread

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
int done = 0;

//parent
pthread_mutex_lock(&mutex);
while (done = 0)
{
	pthread_cond_wait(&cond, &mutex);
	pthread_mutex_unlock(&mutex);
}

//child
pthread_mutex_lock(&mutex);
done = 1;
pthread_cond_signal(&cond);
pthread_mutex_unlock(&mutex);
```

### Producer/Consumer Problem
Scenario: 