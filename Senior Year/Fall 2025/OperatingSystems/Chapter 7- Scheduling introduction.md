Operating systems
09-10-2025

### What is scheduling?
- Scheduling in operating systems is where the OS must decide which process get to use the CPU and at what time.
- Good scheduling from the OS makes the computer runs faster.
---
### Why do we schedule? (Metrics to optimize)
1. Turnaround time
	- Turnaround time is how long it takes for a job to finish after it arrives.
	- The formula is:
``` c
	  completion_time - arrival_time
	  ```

- The goal is to make the turnaround time as short as possible.	 
 
2. Response time
	- Response time is how long it takes from when a job arrives until it first runs.
	- The formula is:
```c
		first_scheduled_time - arrival_time
```
- The goal is to respond quickly to keep users from waiting too long

3. Fairness
	- Fairness in this context is every job should get a fair share of the CPU over time.
	- The issue arises when one job finishes quickly. This can hurt fairness for others
Overall we want to minimize:
- Waiting time
- Turnaround time
- Response Time
- Overhead
And we want to maximize:
- Throughput
- resource utilization
- Fairness
---
### Workload Assumptions (starting simple)
- When studying scheduling we first makes some simple assumptions:
	1. All jobs take the same amount of time
	2. all jobs arrive at the same time
	3. Jobs run until finished once started
	4. Jobs only use CPU (no I/O)
	5. The OS knows how long each job takes
- These assumptions make it easier to study scheduling even though real systems are much much more complex
---
### Scheduling Policies
#### 1. First Come, First Served (FCFS / FIFO)
- Jobs run in the order they arrive
-  Simple and easy to implement
**Example**: we have three jobs, A (10s), B (10s), and C (10s)
	- A runs first, then B, then C.
	- Therefore, the turnaround times are:
		- A = 10s , B = 20s , C = 30s → Average = 20s
- _Problem (Convoy Effect)_:
	- If one long job arrives first, shorter jobs wait too long.    
	- Example: A=100s, B=10s, C=10s → Average turnaround = 110s.
#### 2. Shortest Job First (SJF)
- Always pick the shortest job first
- Non-preemptive (this means once started the task runs until finished)
**Example**: Using the same values from FIFO example: A = 100s , B = 10s , C = 10s
- Order B → C → A.
- Turnaround times B = 10s, C = 20s, A = 120s → Average = 50s
		Note this solution is much better than the 110s from the FIFO example above
#### 3. Shortest Time-to-Completion First (STCF / Preemptive SJF)
- Just like Shortest Job first however it *is* preemptive
- This means that the OS can stop a long job and switch to a shorter one if it so chooses.
**Example**:
	- A = 100s and arrives at t = 0
	- B = 10s and C = 10s arrive at t = 10
	- The scheduler pauses A and runs B, then C, then continues A
	- So the Turnaround time A = 120 , B = 10 , C = 20 → 50s
- The Benefit is that the short jobs get done more quickly
- This method is best for minimizing Turnaround time
**NOTE**:
- **SJF** is good for **batch systems** where all jobs are known upfront. (Example: rendering 100 video files overnight).    
- **STCF** is better for **interactive or mixed systems** where jobs can appear at any time. (Example: your laptop, where you may open a browser while a big update is running).
#### 4. Round Robin
- Each job gets a small time slice (quantum)
- After its slice, the job goes to the back of the line
- This way each process gets time with the CPU for its slice making it more fair
- Turnaround however, is noticeably worse
**Example**:
	- A, B, C each need 5s. Time slide = 1s
	- Order keeps rotating: A → B → C → A → B → C... etc.
	- Response time is good because every job starts quickly
Trade-off:
- **Small time slice** = better response time, but too many context switches are needed (High overhead)
- **Large time slice**  = fewer switches, but worse response time
---
### Incorporating I/O
- Real programs do not just use the CPU but they also use input and output(reading disks, network requests, etc.)
- When a process does I/O, it gest blocked (meaning it cant use the CPU until the I/O finishes)
- The scheduler gives CPU to another job while waiting'
**Example**:
	- A runs 10ms and then does I/O for 10ms
	- B only needs CPU for 50ms
	- OS runs A → then B while A is waiting for I/O → then A again. 
	- This keeps the CPU busy instead of wasting time.
- --
### **Summary**

- **Scheduling** decides which process runs on the CPU.    
- **Metrics:** Turnaround time (finish fast), response time (start fast), fairness (equal share).
- **Policies:**    
    - **FCFS** (First come first served): Simple, but long jobs block short ones.        
    - **SJF** (Shortest jobs first): Shortest jobs get done first, faster average turnaround.       
    - **STCF** (Shortest Time-To-Completion First) :Preemptive SJF, best for turnaround.       
    - **RR** (Round Robin): Fair sharing, better response time, but needs good time slice choice.        
- **I/O matters:** Scheduler must switch jobs when one is waiting on I/O.