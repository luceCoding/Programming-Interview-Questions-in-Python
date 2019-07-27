# 621. Task Scheduler

## Heap solution
- Runtime: O(Nlog(U))
- Space: O(U)
- N = Number of elements in array
- U = Number of unique elements in array

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

```
from collections import Counter

class Solution:
    def leastInterval(self, tasks: List[str], n: int) -> int:
        n_intervals = 0
        ch_to_count = Counter(tasks)
        max_heap = [-count for count in ch_to_count.values()]
        heapq.heapify(max_heap)
        while len(max_heap) > 0:
            popped_items = list()
            for _ in range(n+1):
                if len(max_heap) > 0:
                    popped_items.append(heapq.heappop(max_heap))
                else:
                    break
                
            max_heap += [count+1 for count in popped_items if count+1 != 0]
            heapq.heapify(max_heap)
            
            n_intervals += len(popped_items) if len(max_heap) == 0 else n+1
            
        return n_intervals
```
