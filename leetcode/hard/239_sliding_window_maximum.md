# 239. Sliding Window Maximum

## Best Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

I am not a big fan of invariant problems, but this question was a good solution, something I might see myself using one day.

I am assuming you understand how a sliding window is implemented via. a deque, so I won't go over that portion.
The tough part is the invariant portion of the problem.
Its a little tricky to understand at first.

Some attributes you need to notice are:
- The sliding window always moves forward.
- As long as the max number is within the sliding window, all other previous smaller numbers are obsolete.

By understanding the second bullet, you can come up with a solution yourself.

For example:
```
[1,2,3,-1,2,5]
k=3
```
1. We start with a sliding window of [1,2,3], at this point the deque is [3].
2. When we get to [2,3,-1], the deque is [3,-1]
3. Window = [3,-1,2], Deque = [3,2]
4. Window = [-1,2,5], Deque = [5]

Implementation Steps:
1. We essentially need to maintain a deque of numbers that are in descending order, from left to right.
2. If a given number is larger than the right-most number in the deque, 
we will continue to pop it off until there is nothing left in the deque or we find another number greater than or equal to itself.
This is similar to a stack but as a deque.
3. If the left-most number is outside of the sliding window, we need to pop it off.
4. We take the left-most number in the deque as the result.

```
from collections import deque

class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        if len(nums) == 0 or k == 0:
            return []
        queue = deque()
        result = list()
        for index, num in enumerate(nums):
            if len(queue) > 0 and index - queue[0] >= k: # maintain sliding window
                queue.popleft()
            while len(queue) > 0 and nums[queue[-1]] < num: # maintain decending order of deque
                queue.pop()
            queue.append(index)
            if index >= k-1: # make sure we have seen k elements first
                result.append(nums[queue[0]])
        return result
```
