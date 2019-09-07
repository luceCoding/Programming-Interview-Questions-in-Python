# 128. Longest Consecutive Sequence

## Best Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

The most intuitive solution is to sort the array, then iterate each number to find the longest range.
Hoever, that would be N(log(N)) run-time.

To improve the run-time, we can store all the numbers into a set, then check if the left(n-1) and right(n+1) numbers are in the set.
However, the run-time would be poor(O(N^2)), you would need a visited set to avoid traversing left and right for each number in the set to get O(N).

You can further improve the space complexity by only starting at either the left-most number or the right-most number.
That would mean you just traverse only in one direction, avoiding the need of a visited set.

```
class Solution:
    def longestConsecutive(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        num_set = set(nums)
        longest_range = 0
        for n in num_set:
            if n-1 not in num_set: # beginning of a range
                curr_n = n
                while curr_n+1 in num_set:
                    curr_n += 1
                longest_range = max(longest_range, curr_n-n)
        return longest_range+1
```
