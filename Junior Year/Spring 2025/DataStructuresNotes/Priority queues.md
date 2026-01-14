Data Structures
03-26-2025

>Priority queues can be used in scenarios where you still want a queue but some object within the queue are to be put before others. 

An examples is a line at an amusement park that offers a fast pass which allows the user to skip towards the front of the queue.

For each object pushed we will associate a nonnegative number where 0 is the top priority.

Priority queues visualized:
![](../../../-images/Priority%20queues.png)


One thing to note is due to the nature of a priority queue, every object within the queue is already sorting allowing for O(n) time complexity.

If you were to sort a list into a priority queue, you would take the top/front value first and throw it into the queue and then continue from the front until the list is empty. 
Note sorting like this has the time complexity O(1+2+3+...(n-1)) which works out to O(n^2)