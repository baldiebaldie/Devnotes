Analysis of algorithms
01-21-2026

### Review 
- Asymptotic notations - mathematical tools used in algorithms to tell how time or space complexity grows as the input size grows
- Several ways to analyze an algorithms potential performance, on worst/best/average cases
- Big O focuses on the upper bound (worst case scenario) for an algorithms performance
- Omega notation focuses on the lower bound (the best case scenario) for an algorithms performance

### Omega Notation (exact growth rate function)
- Omega gives lower bound. 
- "This algorithm with take at least this much time"
- f(n) = Ω(g(n))
- i.e
	- f(n) = 100n^2+10n+50, so g(n) is Ω(n^2)
- Mathematically speaking:
	- there exists a constant c where c > 0
### Theta Notation (average growth function)
Theta notation is a tight lower bound function
- Decides whether the upper and lower bounds of a given function are the same
- The average running time of an algorithm are always between the upper and lower bounds.
- If the upper bound is the lower bound of an algorithm are the same, then its growth is exactly θ(g(n))
- i.e
	- f(n) = 2n + 20:
		- n is number of coffee
		- 20 is fixed monthly cost
		- 2 is cost of the coffee
- Worst case is infinity (n grows large)
- best case is 20
- average case is n where cost grows lineally with coffee

### Asymptotic Notations
- When analyzing a function, we are looking to find another function g(n) that approximates how f(n) grows when the input size n becomes very large.
- g(n) is like a curve that follows the shape of f(n) for large n
- In math, we call this an asymptotic curve
- We focus on their growth trends for large inputs ignoring small details and constants
### Guidelines for asymptotic notation
- Loops
	- At most, the running time of the loop is multiplied by the amount of iterations n
	- i.e
``` c
  for (int i = 1; i<n ; i++) {
	do something;  
  }
```
	 This loop time complexity is O(n)

- Nested loop
	- Analyze from the inside out. Total running time is the product of the sizes of all the loops
``` c
  for (int i = 1; i<n ; i++) {
	  for(int j = n; j<m ; j++) {
		  do something;
	  }  
  }
```
	This nested loops time complexity is m*n or O(n^2)

#### Consecutive statements
- Consecutive statements
	- Add the time complexity of each statement
	- i.e
``` c
int main() {
	a = a + 10; //O(1) bc its a constant
	for( int i = 1 ; i<n ; i++) { //O(n) because its a loop
		x = x + 1; //O(1) bc its a constant
	}
	
	return 0;
}
```
- So here you just add up the different complexities 1 + n + 1 which just comes out to O(n)

#### Conditionals
- Worst case running time is the conditional is whichever is larger in the if or the else
- i.e
``` c
if ( n = 0) {
	return false;
}

else {
	for (int i = 0; i<n; i++) {
		if(!condition) {
			do something;
		}
	}
}
```
In this case the else branch has more time complexity and therefore the conditionals time complexity is O(n)

### Logarithmic complexity 
An algorithm is O(log n) if it takes a constant time to cut the size by a fraction (usually 1/2)
- i.e
``` c
int i = 1;
n = 100;

while (i < n) {
	i = i * 2;
}
```

- So here i = 2^k 
- log(2^k)  = log(n)