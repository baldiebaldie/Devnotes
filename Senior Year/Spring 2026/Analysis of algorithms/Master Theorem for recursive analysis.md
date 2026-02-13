Analysis of algorithms
02-11-2026

### MASTER THEORUM:
Format for recursive:
a \* T(n - 1) +f (n)

Output for big O given what a is:
1. a =1
	- O(n * f(n))
2. a>1
	- O(f(n) a ^ (n/b)))
3. a<1
	- O(f(n))

i.e: T(n) = 3T(n-1) + 1
	a: 3
	b: 1
	f(n): 1
	
	So for this problem the time complexity would be
	
	O(1 * 3^(n/1)) = O(3^n)
i.e: T(n) = T(n-1) + n
	O(n * f(n))
	so O(n * 1) = O(n)
	
i.e: T(n) = 2T(n-1) + 1
	O(f(n) * a^(n/b))
	so O(1 * a^(n/1)) = O(2^n)
	
i.e: T(n) = T(n-1) + logn
	O(n * f(n))
	so O(n * logn) = O(nlogn)
	
i.e:  T(n) = 2T(n-1) + n
	O(f(n) * a^(n/b))
	so O(n * a^(n/1)) = O(2n^n) = O(n\*2n) = O(n^2)

- 


