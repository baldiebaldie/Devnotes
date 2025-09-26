Operating systems
09-15-2025
### Basic idea
- Proportional share scheduling is like dividing a pizza fairly among friends. Instead of giving everyone equal slices, you give bigger slices to people who deserve more based on some rule. In computers, this means giving more CPU time to processes that need it more.
- The main goal is to guarantee that each process gets a certain percentage of CPU time. This type of scheduling doesn't care about making processes finish quickly. It only cares about being fair.
---
### Proportional Share Schedulers
- Proportional Share Schedulers guarantee each job a certain percentage of CPU time. This is different from other schedulers because it isn't focused on response time or turnaround time
- An example of this is Lottery scheduling
### Lottery scheduling
- Lottery scheduling uses tickets to decide which process gets to run. Each process is given a number of tickets
- Each process is given a number of tickets. The number of tickets a process has represents the share of the resources it should get. 
- The scheduler picks a a random number, which is the winning ticket. The process holding that ticket is the winner and gets to run one time slice. 
- Note that a process with more tickets have a higher change of winning.
#### Lottery Scheduling example
If we have two processes A and B having 75 and 25 tickets respectively out of total of 100 tickets. CPU  
share of A is 75% and that of B is 25%. These shares are calculated probabilistically and not deterministically.  
Explanation:  
1. We have two processes A and B. A has 75 tickets (tickets 0 to 74), and B has 25 tickets (tickets 75 to 99).  
2. Scheduler picks a random number (winning ticket) from 0 to 99. If the picked no. is from 0 to 74 then A is executed otherwise B is executed.  
3. An example of 24 tickets picked by the Scheduler may look like this follows:
![](../../images/Pasted%20image%2020250915150404.png)
#### Example of code implementation for Lottery scheduling

```c 
1 // counter: used to track if we’ve found the winner yet  
2 int counter = 0;  
3  
4 // winner: use some call to a random number generator to  
5 // get a value, between 0 and the total # of tickets  
6 int winner = getrandom(0, totaltickets);  
7  
8 // current: use this to walk through the list of jobs  
9 node_t *current = head;  
10  
11 // loop until the sum of ticket values is > the winner  
12 while (current) {  
13 counter = counter + current->tickets;  
14 if (counter > winner)  
15 break; // found the winner  
16 current = current->next;  
17 }  
18 // ’current’ is the winner: schedule it...
```

---
### Fairness metric
- The time the first job completes the task divided by the time that the second job completes its task
- i.e.
	- There are two jobs, each job has runtime 10.  
		- First job finishes at time 10  
		- Second job finishes at time 20  
		- Fairness = $10 / 20$ = 0.5 
		- F will be quite close to 1 when both jobs finish at nearly the same time.
---
### Stride Scheduling
- Stride scheduling is a different type of proportional share scheduler. It is deterministic meaning it is NOT based on chance.
- Each Process is assigned a stride value. The stride is calculated by dividing a large number by the number of tickets a process has. For instance, with a large number of 10,000, a process with 100 tickets has a stride of 100. A process with 50 tickets has a stride of 200.
- The scheduler keeps a counter called a *pass value*. It always chooses the process with the lowest pass value to run
![](../../images/Pasted%20image%2020250915151333.png)
#### How it works
1. Each process gets a "stride" value = (Big number) / (# of tickets)
2. Each process has a "pass value" that starts at 0
3. The process with the *Lowest* pass value runs next
4. After running, that process adds its stride to the pass value.
i.e.
- Process A: 100 tickets -> stride = 100
- Process B: 50 tickets-> stride = 200
- Process C: 250 tickets-> stride = 40
Note that process C has the shortest stride and therefore will run the most often. This creates perfect fairness without randomness.
---
### Problem with new processes
- If a new process enters with a pass value of 0, it will monopolize the CPU until its pass value catches up. This is a major flaw.
- --
### Linux Completely fair scheduler (CFS)
- CFS is the real-world scheduler used in Linux and it is highly efficient and scalable.
 - CFS tracks virtual runtime (vruntime) for each process. 
 - you can think of vruntime as a scoreboard showing how much CPU time each process has used. 
 - The process with the lowest score runs next.
 
**Dynamic Time Slices**
- Unlike fixed time slices, CFS calculates time slices dynamically
- It does this by using a parameter called "sched_latency" (usually 48ms)
- Time slice = sched_latency / number of processes
i.e.
- 4 processes:  (time slice = sched_latency / number of processes) : 48 / 4 = 12ms
 
**Minimum Time Slice**
- To prevent too many context switches when there are many processes, CFS sets a minimum time slice (min_granularity = 6ms). 
- Even with 100 processes, no process gets less than 6ms

**Priority with Nice values**
CFS uses "nice" values to give some processes priority:
- Nice values range from -20 to +19 (default is 0)
- Negative values = higher priority and Positive values = lower priority
- CFS converts nice values to weights using a lookup table

**How weights affect scheduling**
- Processes with higher weights get:
- Longer time slices
- Slower-growing vruntime (so they run more often)
i.e.
- If process A has a weight of 3121 and Processes B had a weight of 1024. 
- Time slice = (weight of process / sum of all weights) * sched_latency
- Then A gets (3121/4145) * 48ms = 36.1 ms
- B gets (1024/4145) * 48ms = 11.9 ms.
- 36.1/11.9 = about 3. Therefore process A gets the CPU for about 3 times as long as processes B gets the CPU.

 **Efficient Data Structure**
 - CFS uses a red-black tree (a type of balanced binary tree) to store processes. This makes finding the next process to run very fast - O(log n) time complexity.

**Handling Sleeping Processes**
- When a process wakes up from sleep, its vruntime might be very small compared to the running processes.
- This would let it monopolize the CPU
- CFS prevents this by setting the waking process's vruntime to the minimum value in the tree.
- This means processes that sleep frequently don't get extra CPU time when they wake up. 
Example problem...
**Initial state:**
- Process A (CPU-intensive): vruntime = 100ms
- Process B (I/O process): vruntime = 20ms, then goes to sleep
**After Process A runs for 80ms more:**
- Process A: vruntime = 180ms
- Process B: still vruntime = 20ms (was sleeping)

##### Without CFS Protection
When Process B wakes up, it would have vruntime = 20ms while Process A has vruntime = 180ms. Process B would monopolize the CPU for a long time to "catch up."

##### With CFS Protection
When Process B wakes up, CFS sets its vruntime to the minimum value currently in the red-black tree.

**CFS correction:**
- Process B wakes up with vruntime = min(current active processes)
- If Process A is the only active process with vruntime = 180ms
- Process B gets set to vruntime = 180ms (same as Process A)
 Vruntime wakeup formula
- vruntime wakeup = max(old vruntime, min vruntime in tree)
This ensures that sleeping processes don't fall too far behind, but also don't get unfair advantages when they wake up.

---

### Vruntime Example
- vruntime = vruntime + (weight 0 / weight of process) × actual runtime (where weight of 0 is 1024 [default])
Both processes run for 12ms of actual time:
**Process A (weight = 3121):**
- vruntime A = 0 + (1024 / 3121) × 12ms
- vruntime A = 0 + 0.328 × 12ms = **3.94ms**
**Process B (weight = 1024):**
- vruntime B = 0 + (1024 / 1024) × 12ms
- vruntime B = 0 + 1.0 × 12ms = **12ms**

Process A's virtual runtime grows much slower, so it will be selected to run more often.
 

---
## Key Differences Summary

**Lottery Scheduling:** Uses randomness, can be unfair in short term, simple to implement

**Stride Scheduling:** Perfectly fair and deterministic, but has problems with new processes

**CFS:** Practical real-world solution, highly efficient, handles edge cases well, uses dynamic time slices

The main tradeoff in all these schedulers is between fairness and performance. More frequent switching increases fairness but hurts performance due to context switching overhead.