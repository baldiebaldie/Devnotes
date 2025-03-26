Data Structures
03-26-2025


>Heaps: a binary tree sorting keys at its nodes and satisfying the following properties:
>
- If we have a max heap: key(Parent) >= key(Child)
- If we have a min heap: key(Parent) <= key(Child )

**The last node of a heap is the rightmost node of maximum depth**

What is the appeal of using a heap?
Heaps are great for:

- Finding the minimum or maximum value quickly
- Implementing priority queues
- Sorting algorithms like heap sort
- Has time complexity O(nlogn)  ( *Very fast!!* ) but in its best case is O(logn)

Heap example:
One example would be monitoring stock prices:
Lets say you have a continuous stream of stock prices which update from different sources. You want to track the top K highest-priced stocks at  any given moment. If you wanted to keep it sorted it would be slightly inefficient to use quick sort it would be O(n^2 logn) (this is because it is continuously quicksorting n times hence O(n) * O(nlogn)). In this case we could use heaps

The main operations for heaps are:
- Insert: Add a new element
- Extract: Remove the top element
- Heapify: Create a heap from an array

Heapify specifics:
 ## 2 types of heapify
1. **Heapify Down (Sift Down)**: Start from a parent node and move it down to the correct position
2. **Heapify Up (Sift Up)**: Start from a child node and move it up to the correct position
Heapify compares two values then swap the two accordingly to reach a state where the heap is fully sorted. 

NOTE that fully sorted trees that are used in heaps are top down, greatest to least, and right to left, greatest to least.
Insertion specifics:

Exract specifics:
