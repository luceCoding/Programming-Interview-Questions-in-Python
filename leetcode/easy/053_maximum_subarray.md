# 53. Maximum Subarray

## Kadane's Algothrim
- Run-time: O(N)
- Space: O(1)
- N = Number of nodes in tree

Kadane's Algothrim is a good thing to know.
There are proofs out there explaining why this works if you are interested.
Overall its a dynamic programming solution at its core.

Because the question is asking for a contiguous subarray, we can use the previous sums to find our max sum for a given n.
The main idea is that there are two choices to be made, whether (previous sum + n) or (n) is larger.

```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        if len(nums) == 0:
            return 0
        max_sum, prev_sum = nums[0], nums[0]
        for n in nums[1:]:
            prev_sum = max(n, n+prev_sum)
            max_sum = max(max_sum, prev_sum)
        return max_sum
```
