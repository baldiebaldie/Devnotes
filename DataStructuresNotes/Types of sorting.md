Data Structures
03-03-2025

## Insertion sort
- It is the fastest of the slow approaches and works by
having a loop inside of a loop that looks at two entries which are next to each other and compares them. If the second element belongs where the first goes, then swap the two. The worst case time complexity would be O(n^2).

The code implementaion looks like this

``` cpp
void insertionSort(int arr[], int n)
{
    for (int i = 1; i < n; ++i) {
        int key = arr[i];
        int j = i - 1;
        /* Move elements of arr[0..i-1], that are
           greater than key, to one position ahead
           of their current position */
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j = j - 1;
        }
        arr[j + 1] = key;
    }
}   
```

Pros

* Simple to implement and understand
* Efficient for small datasets (< 20 elements)
* Adaptive: O(n) time when nearly sorted
* In-place: only requires O(1) extra space
* Stable: preserves order of equal elements
* Online: can sort as data arrives

Cons

* O(n²) time complexity makes it inefficient for large datasets
* Significantly slower than more advanced algorithms for large inputs

Example:

![insertion sort](/images/insertionSort.png)


## Merge sort
1) Merge sort works by splitting the array into two equal halves 
2) Then the algorithm sorts the two smaller trees
3) Then they combine the two sorted arrays

Time complexity is O(n log n)

This is intuitive because the alogrithm splits the work into two n amount of times thus n log n.

The code implementation looks like this

``` c++
void merge(int arr[], int left, int mid, int right) {
    // Calculate sizes of the two subarrays
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    // Create temporary arrays
    int* L = new int[n1];
    int* R = new int[n2];
    
    // Copy data to temporary arrays
    for (int i = 0; i < n1; i++)
        L[i] = arr[left + i];
    for (int j = 0; j < n2; j++)
        R[j] = arr[mid + 1 + j];
    
    // Merge the temporary arrays back into arr[left..right]
    int i = 0;    // Initial index of first subarray
    int j = 0;    // Initial index of second subarray
    int k = left; // Initial index of merged subarray
    
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        } else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    
    // Copy remaining elements of L[] if any
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }
    
    // Copy remaining elements of R[] if any
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
    
    // Free allocated memory
    delete[] L;
    delete[] R;
}

void mergeSort(int arr[], int left, int right) {
    if (left < right) {
        // Find the middle point
        int mid = left + (right - left) / 2;
        
        // Sort first and second halves
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        
        // Merge the sorted halves
        merge(arr, left, mid, right);
    }
} 
```



The visual representation looks like this:

![merge Sort](/images/MergeSort.png)

Pros

-    Consistent Performance: O(n log n) time complexity in all cases (best, average, worst)
- Stability: Preserves the relative order of equal elements
- External Sorting: Works well with external storage when dealing with large datasets
- Parallelization: Can be easily parallelized due to its divide-and-conquer nature
- Linked Lists: Very efficient for linked lists (requires only pointer manipulation, not extra space)
- Predictability: Performance doesn't depend on input data distribution

Cons

- Extra Space: Requires O(n) additional space for the temporary arrays
- Not In-Place: The standard implementation is not in-place
- Not Adaptive: Doesn't take advantage of partially sorted arrays
- Not Cache-Friendly: Can cause more cache misses than algorithms like quicksort
- Function Call Overhead: Recursive implementation has function call overhead
- Overkill for Small Arrays: For small arrays, simpler algorithms like insertion sort are faster



|      | Insertion sort | mMrge sort   |
| :---        |    :----:   |          ---: |
| Speed      | - fast for small sets O(n^2)      |- fast for overall O(n log n)  |
| Ease of use   | -  easy to implement        | - more difficult to implement  |

**The takeaway form these two sorting alogrithms so far is that insertion sort is earier to implement and better for smaller arrays while merge sort might be more difficult to implement but is significantly better/faster for larger data sets**


## Quick Sort [sort()]
Quicksort takes a divide and conquer approach to sorting.

1) sort() first selects a pivot, or an element to use.
2) Then the algorithm partitions the array into sections around the given pivot. The smaller elements will go to the left of the pivot and the larger elements go to the right of the pivot.
3) Then sort() applies the above steps recursively to the smaller arrays.

In other words, a pivot is selected and two subsequent sub arrays are made with the elements that are smaller than the pivot and one larger. Then the algorithm does this again recursively for each and every sub array until the array is fully sorted.

The time complexity of this alogrithm is O(n log n)

if you #include alogrithm then you can use quicksort like sort(). All you need to do is make a comparison funciton.

The comparison function looks like this:
``` cpp 
bool compare(int a, int b) 
{
    return a > b; // Sort in descending order
}

// Using function pointer
sort(vec.begin(), vec.end(), compare);

```

Visually the alogrithm looks like this: 

![quick sort](/images/quickSort.png)


Pros

- Efficiency: O(n log n) average time complexity, often fastest in practice
- In-Place: Requires only O(log n) extra space for the recursion stack
- Cache Friendly: Works well with memory hierarchies due to good locality of reference
- Adaptive: Performance improves for partially sorted inputs with proper pivot selection
- Parallelizable: Can be implemented as a parallel algorithm
- Tail-Recursion: Can be optimized to use tail recursion
- Divide-and-Conquer: Breaks problem into smaller subproblems

Cons

- Worst-Case Performance: O(n²) time complexity in the worst case (when poorly pivoted)
- Not Stable: Does not preserve the relative order of equal elements
- Sensitive to Pivot Selection: Performance heavily depends on the choice of pivot
- Not Ideal for Linked Lists: Unlike merge sort, not efficient for linked lists
- Recursive Nature: Deep recursion can cause stack overflow for large datasets
- Not Well-Suited for Tiny Arrays: Insertion sort outperforms it for very small arrays


On the exam it is likely we will be given an example array of say 5 elements and give the pivot element and we will have to manually perform quick sort.

A great website to visuallty see this algorithm as well as practice for that question on the exam:  [www.hackerearth.com](https://www.hackerearth.com/practice/algorithms/sorting/quick-sort/visualize/?arrayValues=1%2C+3%2C+4%2C+8%2C+23)

#
#

## 3/06/25 Additional sorting algorithm notes
[Youtube reference video](https://www.youtube.com/watch?v=rbbTd-gkajw)

## Bubble sort
- easiest to implement
- works by taking the first two indexs and compares the two. If the value rightmost index is more than the left one then they swap. It then moves one index over each
- repeats until fully sorted
---
-- i.e --

2, 1, 5, 3, 4

^   ^ (2 !< 1 so swap)

1, 2, 5, 3, 4

--^ ^(2 < 5 so stay) 

1, 2, 5, 3, 4

-----^ ^ (5 !< 3 so swap)

1, 2, 3, 5, 4

-------^ ^(5 !< 4 so swap)

1, 2, 3, 4, 5

Sorted!!

---
  
- Has terrible performance
- The time complexity is O(n^2)

## Selection sort

- Known as an in place comparsion sorting algorithm
- Divideds unsorted array into a sorted sublist
- The algorithm goes through the entire list until it finds the smallest value, and swaps it with the value at the front of the list.
- Does this until entire list is sorted

---
Selection Sort Example:

5, 3, 1, 4, 2
^ -------> (find min in whole array: 1)

^ (swap 5 with 1)

1, 3, 5, 4, 2

  ^ ------> (find min in remaining array: 2)

  ^ (swap 3 with 2)

1, 2, 5, 4, 3

    ^ ----> (find min in remaining array: 3)

    ^ (swap 5 with 3)

1, 2, 3, 4, 5

      ^ --> (find min in remaining array: 4)

      ^ (already in place, no swap)

1, 2, 3, 4, 5

        ^ (last element, already sorted)
        
1, 2, 3, 4, 5

Sorted!!

---

- The time complexity for this is also O(n^2) [not very good]


## Insertion sort

- similar to selection sort 
- Creates 2 arrays, 1 sorted, 1 unsorted
- Goes through elemenets 1 by 1 in the unsorted array 
- It iterates over each number to see if the next is between them.

---

Insertion Sort Example:

5, 3, 1, 4, 2

^ ^ (compare 3 with 5, 3 < 5 so insert 3 before 5)

3, 5, 1, 4, 2

--^ ^ (compare 1 with 5, 1 < 5 so continue)

^ ^ (compare 1 with 3, 1 < 3 so insert 1 before 3)

1, 3, 5, 4, 2

----^ ^ (compare 4 with 5, 4 < 5 so insert 4 before 5)

1, 3, 4, 5, 2

------^ ^ (compare 2 with 5, 2 < 5 so continue)

----^ ^ (compare 2 with 4, 2 < 4 so continue)

--^ ^ (compare 2 with 3, 2 < 3 so continue)

^ ^ (compare 2 with 1, 2 > 1 so insert 2 after 1)

1, 2, 3, 4, 5

Sorted!!

---

- Also has time complexity O(n^2) however is more desirable than both selection sort and bubble sort
- It is a great option for very small lists but extremely impracticle for much larger lists

## Merge sort
- comparison based algorithm
- one of the divide and conquer algorithm

1) splits array into equal chunks again and again
2) Then merge the sub arrays into other sub arrays and compare their contents.
3) continue doing this until all of the sub arrays are made back into the original array


---

Merge Sort Example:

Original array:

5, 3, 1, 4, 2

Divide into subarrays:

[5, 3] [1, 4, 2]

Further divide:

[5] [3] [1] [4, 2]

Further divide:

[5] [3] [1] [4] [2]

Merge [5] and [3]:

^ ^ (compare 5 with 3, 3 < 5)

[3, 5]

Merge [1] and [4]:

^ ^ (compare 1 with 4, 1 < 4)

[1, 4]

Merge [1, 4] and [2]:

^ --^ (compare 1 with 2, 1 < 2)

[1] [2, 4]

--^ ^ (compare 4 with 2, 2 < 4)

[1, 2, 4]

Merge [3, 5] and [1, 2, 4]:

^ ----^ (compare 3 with 1, 1 < 3)

[1] [3, 5] [2, 4]

--^ ----^ (compare 3 with 2, 2 < 3)

[1, 2] [3, 5] [4]

----^ --^ (compare 3 with 4, 3 < 4)

[1, 2, 3] [5, 4]

------^ ^ (compare 5 with 4, 4 < 5)

[1, 2, 3, 4, 5]

Sorted!!

---

- The time complexity of this is O(nlogn)

## Quick sort
- Similar to merge sort but needs a pivot
- Note there are many ways to choose the pivot
- in the example below, the pivot is chosen as the rightmost value
- Sub arrays are then made around the pivot (one sub array with values that are less than the pivot) (One where values are larger than the pivot)
- Then a pivot point is chosen in the sub list and then the process repeats recursively until the entire list is sorted

---

Quick Sort Example:

Original array:

5, 3, 1, 4, 2

Choose pivot (rightmost element):

5, 3, 1, 4, 2

----------^ (pivot = 2)

Partition:

5, 3, 1, 4, 2

^ (5 > 2, move to right)

5, 3, 1, 4, 2

--^ (3 > 2, move to right)

5, 3, 1, 4, 2

----^ (1 < 2, stay on left)

1, 3, 5, 4, 2

------^ (4 > 2, move to right)

1, 3, 5, 4, 2

--------^ (pivot, swap with rightmost unsorted)

1, 2, 5, 4, 3

  ^ (pivot position)

Recursively sort left subarray [1]:

1

^ (already sorted, single element)

Recursively sort right subarray [5, 4, 3]:

5, 4, 3

----^ (pivot = 3)

Partition:

5, 4, 3

^ (5 > 3, move to right)

5, 4, 3

--^ (4 > 3, move to right)

5, 4, 3

----^ (pivot, swap with rightmost unsorted)

3, 4, 5

^ (pivot position)

Recursively sort left subarray []:

(empty, nothing to sort)

Recursively sort right subarray [4, 5]:

4, 5

--^ (pivot = 5)

Partition:

4, 5

^ (4 < 5, stay on left)

4, 5

--^ (pivot position)

Combine all sorted partitions:

1, 2, 3, 4, 5


Sorted!!

---

- The time complexity is O(n^2)
