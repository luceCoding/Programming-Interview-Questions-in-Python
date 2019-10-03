# 295. Find Median from Data Stream

## Sort solution
- Runtime: O(log(N)) for addNum() and O(1) for findMedian(), in total O(Nlog(N))
- Space: O(N)
- N = Number of elements in array

This solution is fairly simple, as long as we keep a sorted order of numbers, we can figure out the median quickly.

However, it is important to note that if we instead did a different approach where we would add the value into the list then sort it only when findMedian() was called, it would actually cause a slower run-time, O(N * Nlog(N)).
The worst case is when we call findMedian() after each newly inserted number.
That is because every time we would sort, the list keeps growing, we aren't utilizing the fact that the list is already sorted.
With an already sorted list, we can just perform a binary search and insert the new number instead of resorting an already sorted list for each number.

```
import bisect

class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self._nums = list()

    def addNum(self, num: int) -> None:
        bisect.insort(self._nums, num)

    def findMedian(self) -> float:
        if len(self._nums) == 0:
            return 0
        median_index = len(self._nums) // 2
        return self._nums[median_index] if len(self._nums) % 2 \
            else (self._nums[median_index-1] + self._nums[median_index]) / 2
```

## Two Heap Solution
- Runtime: O(log(N)) for addNum() and O(1) for findMedian(), in total O(Nlog(N))
- Space: O(N)
- N = Number of elements in array

This second approach is rather innovative to say the least.
It uses two heaps, one that keeps a set of large numbers and another set of smaller numbers.
You can think of this as a divide and conquer approach.

The only tricky part is to keep the two heaps balanced, balanced meaning the two heaps cannot differ in size by more than 1.
Secondly, we need to keep the two heap property of smaller and larger sets.

Once these two properties are met, finding the median can be done by using the two values on top of the heap if both heap sizes are the same or taking the top value of the larger heap.

```
class MedianFinder:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.max_heap = list()
        self.min_heap = list()

    def addNum(self, num: int) -> None:
        heapq.heappush(self.max_heap, -heapq.heappushpop(self.min_heap, num))
        if len(self.max_heap) > len(self.min_heap):
            heapq.heappush(self.min_heap, -heapq.heappop(self.max_heap))

    def findMedian(self) -> float:
        return (self.min_heap[0] + -self.max_heap[0]) / 2 \
            if len(self.min_heap) == len(self.max_heap) else self.min_heap[0]
```
