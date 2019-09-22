# 767. Reorganize String

## Greedy Heap Solution
- Runtime: O(Nlog(N))
- Space: O(N)
- N = Number of characters in string

Playing around with different examples like 'aabb', 'aabbc', 'aabbcc' or 'aaabbc'.
We can see a pattern where a greedy approach can be taken.
We can build the string by taking the most occurring element.
This leads us to using a max heap to determine this.
We will need a dictionary to count the occurrences and use that to store tuples of (occurrences, character) pairs into the heap.

Another thing we notice is that there can be a scenario where the most occurring element on top of the heap is the same as the last character we just used to build the string, for example, 'aaaabb'. This means that we need to pop two elements from the heap to guarantee that we can use one of these characters.

Last case is when we cannot build a valid string.
This can be determined if the occurrence of the last element in the heap is of one or not after the above sub-solution has be done processing and created the longest valid string possible.

```
from collections import Counter

class Solution:
    def reorganizeString(self, S: str) -> str:
        counter = Counter(S)
        max_heap = list((-v, k) for k, v in counter.items())
        heapq.heapify(max_heap)
        str_builder = list()
        while len(max_heap) >= 2:
            val1, ch1 = heapq.heappop(max_heap)
            val2, ch2 = heapq.heappop(max_heap)
            if len(str_builder) == 0 or (len(str_builder) and str_builder[-1] != ch1):
                str_builder.append(ch1)
                if val1 != -1:
                    heapq.heappush(max_heap, (val1+1, ch1))
            else:
                heapq.heappush(max_heap, (val1, ch1))
            if len(str_builder) and str_builder[-1] != ch2:
                str_builder.append(ch2)
                if val2 != -1:
                    heapq.heappush(max_heap, (val2+1, ch2))
            else:
                heapq.heappush(max_heap, (val2, ch2))
        if len(max_heap): # last node in heap
            val, ch = heapq.heappop(max_heap)
            if val != -1:
                return ''
            else:
                str_builder.append(ch)
        return ''.join(str_builder)
```
