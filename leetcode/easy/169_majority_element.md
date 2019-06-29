# 169. Majority Element

## Solution with dictionary
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

Fairly straight forward, use a dictionary to count the number of occurrences for each number. Then compare it to the current best.

```
from collections import defaultdict

class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        counter = defaultdict(lambda: 0)
        curr_major, curr_count = None, 0
        for n in nums:
            counter[n] += 1
            if counter[n] > curr_count:
                curr_major = n
                curr_count = counter[n]
        return curr_major
```

## Solution with constant space
- Runtime: O(N)
- Space: O(1)
- N = Number of elements in array

Keeping a dictionary of occurrences when you just want to find one major element is performing extra work.
You can instead pass through the array once while keeping track of your current major number and any other number is a reason your current major number should be decremented, else increment it.
Once your current major number's count reaches zero, then it is a reason a new major number takes its place.

```
class Solution:
    def majorityElement(self, nums: List[int]) -> int:
        curr_major, curr_count = None, 0 
        for n in nums:
            if curr_major != n:
                curr_count -= 1
            else:
                curr_count += 1
            if curr_count <= 0:
                curr_major, curr_count = n, 1
        return curr_major
```
