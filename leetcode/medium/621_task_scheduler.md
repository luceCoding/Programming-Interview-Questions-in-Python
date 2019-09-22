# 621. Task Scheduler

## Heap solution
- Runtime: O(N) or O(N(log(26))
- Space: O(1) or 26
- N = Number of elements in array

This question requires a greedy algothrim.
We want to use the task that occurs the most first so we can reduce the amount of idle time there is.
With that, a max heap can help us find those set of tasks.

However, there is one tricky edge case that I've noted below.
If we had an input with many tasks that occur the same amount of times and one that occurs many times. 
It is actually bad to start all the tasks in the first go around.
Better to interweave them between the one task that occurs many times to reduce idle times.

**Important edge case to consider**
```
Input:
["A","A","A","A","A","A","B","C","D","E","F","G"]
2

Wrong Answer:
ABCDEFGA--A--A--A--A
20

Correct Answer:
ABCADEAFGA--A--A
16
```

We can first count the occurances for each character and place them into a heap.
Each element in the heap will represent a different character of A-Z.
We can then pop off at most N+1 amount of items from the heap.
Each popped off item will have their occurances decremented and placed back into the heap if non-zero.
We can repeat this process until there is nothing left in the heap.

Since we will have at most 26 characters in the heap, due to the restriction of having only a A-Z character range.
We can then assume that sorting the heap will have a constant run-time of O(log(26)) or O(1).
However, we will have sort the heap O(N) times for each element in the input.
You can also think of it this way, since the heap will hold just occurances of each letter, the occurances all add up to N elements of the input array.

```
from collections import Counter

class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        counter = Counter(tasks)
        max_heap = list([-freq for freq in counter.values()])
        heapq.heapify(max_heap)
        n_intervals = 0
        while len(max_heap):
            popped_items = list()
            for _ in range(min(len(max_heap), n+1)):
                popped_items.append(heapq.heappop(max_heap))
            for freq in popped_items:
                if freq != -1:
                    heapq.heappush(max_heap, freq+1)
            n_intervals += n+1 if len(max_heap) else len(popped_items)
        return n_intervals
```
