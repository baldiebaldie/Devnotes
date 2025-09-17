Operating systems
09-15-2025

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