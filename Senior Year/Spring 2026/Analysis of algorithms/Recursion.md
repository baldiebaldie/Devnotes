Analysis of algorithms
02-02-2026

Linear recursion has
- space complexity of O(n)
- time complexity of O(n)
To prove linear recursion
- We have to do proof by induction

1. Prove the base case
	- Take the formula and prove that it works as intended when n= 1
2. Prove that it works with any value
	- Assume that your formula works when n = k 
	- Thus it would work if n = k+1
	- Prove that is the case through logic
You can illustrate recursion through a recursive trace
- Write a box for each recursive call
- An arrow for each caller to callee
- An arrow from each callee showing the return value
![](../../../Pasted%20image%2020260202114020.png)

> [!NOTE]
> The factorial function is important because it is known to equal the number of ways where n district items can be arranged in a sequence, that is, the number of permutations of n items

### Tail Recursion
- Happens when a linearly recursive method makes its recursive call as its last step
- i.e
	- The array reversal method

	- Iterative reverse array (A, i, j)
	- Input: An array A of non negative numbers and indices i and j 
	- Output: The reversal of the elements in A starting at index i and ending at j
``` c
while i < j do
		Swap A[i] and A[j]
		i = i + 1
		j = j - 1
	return
````
- These methods are easily converted into iterative functions'

### How to approach writing recursive functions
1. Identify the base case
	- Most important part
	- The base case is the simplest possible version of the problem that you can answer immediately. Without the base case, the recursive function will run forever and crash
2. Identify the recursive step
	- The recursive step is when the function calls itself. You must find a way to break the big problem into slightly smaller versions of the same problem.
	- i.e
		- If we had to solve for n, first find the answer for n - 1
3. Trust the "Recursive leap of faith"
	- Assume the function already works for smaller versions.
	- We do not need to visualize every step the computer takes
4. Combine the results
	- Once we have the answer from the smaller version, you often need to do one last math operation or action to get the final result.
Example: 
- Finding the Fibonacci sequence given the nth iteration
1. Identify the base case
	- The first two numbers are special because they do not have two numbers to add up
		- The 0th number is 0
		- The 1st number is 1
		- If the position is 0, return 0. If the position is 1 return 1
2. Identify the recursive step
	- To find any other number in the sequence you need to know the two numbers that came right before it.
	- This would be n-1 and n-2
3. Trust the leap of faith
	- We assume the function already knows how to find those two previous numbers. We do not worry about the math inside the calls, we wait for them to give the answer
4. Combine the results
	- Lastly we do the simple addition, we add the two smaller numbers we got and add them together
``` c
	void fib(k) {
	//1. Base cases
		if(k == 0) {
			return 0;
		}
		
		else if( k==1 ) {
			return 1;
		}
	//2, 3, & 4
		else {
			return (fib(k-1) + fib(k-2));
		}
	
	}
```


### Tower of Hanoi problem

1. Solving the base case
	- If you have 1 disk, just move it from the starting rod to the destination rod
2. Recursive case
	- If you have a stack of n disks, you cant move them all at once. We have to move the pile out of the way first
		- The plan: move n disks from rod A to rod C
	1. Move the top n-1 disks from rod A to the extra rod B
	2. Move the single biggest disk left on rod A to rod C
	3. Move the n-1 disk from the extra rod B to rod C
3.  Trust the Leap of faith
	- Assume the function already knows how to move a smaller pile. We don't need to watch every single move, we just assume n-1 will successfully move that pile to the spare rod
4. Combine the results
	- The combination here is a sequence of move. You perform the first recursive move, then move the big bottom disk, then perform the second recursive move
- ---
### Recurrence relation
