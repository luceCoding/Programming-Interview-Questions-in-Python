# 128. Longest Consecutive Sequence

## Best Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

The most intuitive solution is to sort the array, then iterate each number to find the longest range.
However, that would be N(log(N)) run-time.

To improve the run-time, we can store all the numbers into a set, then check if the left(n-1) and right(n+1) numbers are in the set.
However, the run-time would be poor, O(N^2), you would need a visited set to avoid traversing left and right for each number in the set to get O(N).

You can further improve the space complexity by only starting at either the left-most number or the right-most number.
That would mean you just traverse only in one direction, avoiding the need of a visited set.

```
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        num_set, longest_length = set(nums), 0
        for n in nums:
            if n-1 not in num_set: # left-most number
                length, curr = 0, n
                while curr in num_set:
                    curr, length = curr+1, length+1
                longest_length = max(longest_length, length)
        return longest_length
```
