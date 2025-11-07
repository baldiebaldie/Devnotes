Operating systems
11-03-2025

Semaphore works as both locks and a conditional variable

**Declaration and Initialization:**
```c
#inlude <semaphore.h>

sem_t sem;
sem_init(sem_t, int pshared, unsigned int value);
```

- There is a command 
	`int sem_wait(sem_t *sem)`: lock a semaphore or wait
	- Decrements sem value by 1. 
	- Waits if value of sem is negative
	- When negative, sem value is equal to number of waiting threads
- There is also
	`int sem_post(sem_t *sem):`: Release or signal a semaphore
	- Increments sem value by 1, then wake as single waiter if exists

### LLM explaining what a semaphore is:
A semaphore is like a ticket system that controls how many people (or programs) can use something at the same time.

**Think of it like a parking lot:**
Imagine a small parking lot with only 3 spaces. There's a ticket booth at the entrance with exactly 3 tickets.

- When you want to park, you must take a ticket first
- If there are tickets available, you get one and can park
- If all tickets are taken, you have to wait in line until someone leaves
- When someone leaves the parking lot, they return their ticket to the booth
- Now the next person in line can take that ticket and park

**How this works in computers:**
The "parking spaces" are shared resources like:

- A printer that can only print one job at a time
- A file that only one program should edit at once
- A database that can handle only 5 connections

The "tickets" are the semaphore count. The semaphore keeps track of how many resources are available.

**Two main operations:**

1. **Wait (or "down")**: "Can I have a ticket please?"
    - If tickets available: take one and continue
    - If no tickets: wait in line
2. **Signal (or "up")**: "Here's my ticket back"
    - Return the ticket so someone else can use it

**Example:** Say you have a printer (1 resource). The semaphore starts with count = 1.

- Program A wants to print: semaphore becomes 0, Program A prints
- Program B wants to print: semaphore is 0, so Program B waits
- Program A finishes: returns ticket, semaphore becomes 1
- Program B can now print

It's basically a fair way to share limited resources without chaos!

#### Negative semaphore

**How a semaphore becomes negative:**

Think back to our parking lot, but now imagine the ticket booth works differently. Instead of just saying "no tickets available," it actually keeps track of how many people are waiting in line.

Here's what happens:

**Starting situation:** Semaphore count = 1 (one parking space)

1. **Person A arrives:** Takes the ticket, count becomes 0
2. **Person B arrives:** No tickets left, but instead of just waiting, the system records this. Count becomes -1
3. **Person C arrives:** Also has to wait, count becomes -2
4. **Person D arrives:** Count becomes -3

**What the negative number means:**

The negative number tells you **how many processes are currently blocked and waiting**.

- Count = -1 means 1 process is waiting
- Count = -2 means 2 processes are waiting
- Count = -3 means 3 processes are waiting

**When someone leaves:**

When Person A finishes parking and returns their ticket:

- Count goes from -3 to -2
- Person B (first in line) gets to park
- 2 people are still waiting

**Why this is useful:**

The operating system can see exactly how many processes are backed up waiting for a resource. This helps with:

- Scheduling decisions
- Detecting potential problems
- Managing system performance

**Real example:** If a semaphore for a printer shows -5, the OS knows 5 programs are waiting to print. It might decide to start up a second printer or warn users about the delay.

So negative semaphores = a waiting list with a counter!

