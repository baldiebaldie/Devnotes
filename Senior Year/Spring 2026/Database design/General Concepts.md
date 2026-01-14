Database Design
01-14-2026
### What can a DBMS(data base management system) do?
Concurrency control has two problems
1. Crash recovery
	- All computer systems will crash 
	- better systems will crash less often
	**DBMS has a recovery requirement where NO data is lost during a crash, it is very good at this because it HAS to**
2. Concurrent possession
	- DBMS allow for multiple changes to the same entries to occur seemingly simultaneously
	- For example, two people sharing the same bank account trying to buy things around the same time. DBMS allows both transactions to come through while still reflecting the correct data.
- ---
### File based system (OS) v.s. DBMS
>**Amount of ✓ indicates how well one system fairs at a specific task**

| Task                           | OS  | DBMS |
| ------------------------------ | --- | ---- |
| Correctness                    | ✓✓  | ✓✓   |
| Efficiency                     | ✓   | ✓✓✓  |
| Concurrency control            | ✓   | ✓✓✓  |
| Data security                  | ✓   | ✓✓✓  |
| Application development (time) | ✓   | ✓✓✓  |
| Portability                    | ✓✓✓ | ✓    |
