## Heap Solution

- Runtime: O(Nlog(K))
- Space: O(K)
- N = Number of elements in array
- K = K frequent elements

We can first iterate through the numbers and count their occurances, we can store this into a dictionary, key: number and value: occurance.
Then we can iterate a second time but with the dictionary.
Since the question wants to know the K most frequent elements, we can sort them, but instead of sorting the entirety of the dictionary elements, we can just sort K amount.
This can be achieved by using a heap of K size.
If we find an occurance that is greater than what is on top of the heap, we can pop it off and add our new element.
This means a min heap would be great for this.

```
from collections import Counter

class Solution:
    def topKFrequent(self, nums: List[int], k: int) -> List[int]:
        counter_map = Counter(nums)
        min_heap = list()
        for num, counter in counter_map.items():
            if len(min_heap) == k:
                heapq.heappushpop(min_heap, (counter, num))
            else:
                heapq.heappush(min_heap, (counter, num))
        return map(lambda x: x[1], min_heap)
```
