Analysis of algorithms
01-12-2026

#### What is an algorithm?
- Defining and solving a problem during the development stage
- make sure it is doable
- do not worry about hardware requirements
- An algorithm writes out the steps from problem statement to result
- independent of any program requirements
- Can be done logically or mathematically
#### What is a program
- Solving the problem during the implementation stage
- consider all of the hardware requirements
---
### Features
Most important features of algorithms are:
- Should know the amount of inputs
- Should be well defined 
- Should take finite time
- should be effective
---
### Analysis of algorithms
Why do we need to analyze algorithms
- Helps us determine which algorithm is better suited for a problem
- Algorithm analysis: study of an algorithms functionality in great detail and how the results are obtained
- Time and space complexity (how much time it takes and how much space it takes):
	- Both are functions f(n)
	- These show if an algorithm are time efficient and/or space efficient
- Hardware cost

---
### How to compare algorithms?
How can you compare two different algorithms?
-  Ideal solution 
	- Express the run time of the algorithm as a function of the input size and compare. Like f(n)
	- The reason this works is because its independent of:
		- Computer speed
		- programming language
		- coding style
- Number of statements to write algorithm
	- Not a great measure since the number of statements might be different between programming language
- Execution time
	- Also not a good measure, this could vary based off hardware and in different environments
---

### How to write an algorithm
You must determine the problem statement. Try to be as consise
When writing an algorithm you must ALWAYS consider what might go wrong first
Analyze how efficient your algorithm is

Requirements
- must have finite steps
 - correctness (does algorithm give solution in finite steps)
 - algorithm must finish in finite time and take finite space
---


- Multiple different algorithms can solve the same problem

- Algorithm analysis helps us see which one is more efficient with space and time taken

---
### Goal of analysis of algo
Goal is to compare algorithms in terms of :
- Runtime
- memory
- developer effort
- etc.
---
### What is the running time analysis 
Running time analysis is:
- How much processing time increase with size of the problem (input) increase
Common types of inputs
- size of array
- Polynomial degree
- number of elements in a matrix
- number of bits 
- vertices and edges in a graph
- etc.

---
### Growth rate
- Growth rate - the rate at which the running time increases as a function given the input size (n) increases.
- In algorithm analysis you can write out the growth rate as a function with multiple terms
	i.e
	- f(n) = n^4 + 2n^2 + 100n + 500
	- Since the values with lower degree than the highest (n^4) they are considered insignificant.. If n were to increase the rate at which n^4 increases completely overshadows the other values within the function. Thus we can rewrite the function as
	- f(n) = n^4
	![](../../../-images/Pasted%20image%2020260114114433.png)
	![](../../../-images/Pasted%20image%2020260114114443.png)
With this in mind there are a few ways to represent an algorithm using expressions
- The best case - the input where the algorithm takes the shortest time
- The worst case - the input where algorithm takes the longest time
- The average time - run the algorithm many times and take the average amount of time

---
### Asymptotic notation
- Having expression for best, average, and worst cases which finds the upper and lower bounds
#### Big-O notation
- Upper bounding function
- Represented as f(n) = O(g(n))
	- This means at larger values the upper bound of f(n) is g(n)
- IT DOES NOT TELL YOU
	- exact time
	- seconds or ms
- But IT DOES TELL YOU 
	- scalability
	- growth

#### Big-O examples
A. Taking attendance
- You have 10 students
	- linear equation which scales linearlly with the amount of student
- Big-o = O(10) = O(1) beacuse it is linear
B. Each student you compare their name with every other student
- If you have 10 students each student with have 10 interaction total
	- With 10 students you will have 100 comparisons total
	- With 100 students you will have 10,000 comparison.
- Big-o = O(n^2) because it is in a nested loop. MUCH faster growth