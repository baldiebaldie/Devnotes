Operating systems
09-15-2025

### Multi-Level Feedback Queue
- A MLFQ is a type of scheduler which are designed to handle two types of jobs without knowing their run times in advance.
- The goal of a MLFQ is to handle two types of jobs
	1. Interactive programs4
		- Needs a fast response time
	2. Batch Programs
		- Concerned with finishing quickly no matter how long they take.
---
### How MLFQs Work

- The MLFQ uses multiple queues. Each queue has a different priority level. A job that is ready to run is place in one of these queues.
	- If a job in a higher queue is ready, then it runs first
	- If two jobs are in the same queue, then they take turns running using round robin scheduling.
- The MLFQ changes a jobs priority based on how it behaves. If a job frequently gives up the CPU to wait for I/O, then its priority stays high to get it done and out of the way. If a job uses the CPU for a long time, then its priority is lowered.
- Several rules that MLFQs follow
	1. If job 1 has higher priority than job 2, have job 1 run first
	2. If job 1 has the same priority has job 2, run them through round robin
	3. When a job starts, its put in the highest priority queue
	4. If a job uses its whole time slice, its priority is lowered and it moves down the queue
		- If a job gives up the CPU before its time slice ends, it stays the same priority level.
- This way, the MLFQ favors shorter jobs very similar to shortest-job-first scheduling
---
### Problems with MLFQ
- There are a few problems with the MLFQ
- Starvation: Long-running jobs might never get to run if there are too many short interactive jobs
- Gaming the scheduler: A job can get a higher share of CPU time by giving up the CPU just before its time slice ends.
### Solution to MLFQ problems
- Priority boost (Rule # 5): All jobs are moved to the highest priority queue after a set time period, S. This helps out long-running jobs (Starved jobs) get another change to run and prevent starvation
- Better accounting: A rewritten rule 4 prevents gaming. A job's priority is lowered once it uses up its total time allotment at a certain level, regardless of how many times it gives up the CPU.
--- 
### Fine-Tuning MLFQ
- Scheduler performance can be improved by adjusting time slices for different queues
	- High-Priority queues have shorter time slices, like 10ms or less.
	- Low-priority queues have longer time slices, like 100ms
- This makes sure interactive jobs get fast response time. Also makes sure long-running jobs don't have to switch between queues too often.