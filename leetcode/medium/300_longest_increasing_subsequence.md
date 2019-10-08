# 300. Longest Increasing Subsequence

## Iterative Solution
- Runtime: O(N^2)
- Space: O(N)
- N = Number of elements in array

If we were to start from the left to the right, we would have seen the longest subsequence on the left side as we are going to the right.
Inorder for us to know what those longest subsequences were, we will need a way to store that, hello dynamic programming. 

For each element, we would need look at the numbers on the left that are less than the current element.
Now that we know which numbers those are, we can then look at their corresponding longest subsequence in the dynamic programming array.
From this list, get the longest subsequence.

We are basically building the longest increasing subsequence from the bottom up.

**Example:**
```
Input(I): [10,9,2,5,3,7,101,18]

DP: 
10   [1, 1, 1, 1, 1, 1, 1, 1]
9    [1, 1, 1, 1, 1, 1, 1, 1] -> DP[1] = 1 + max([]) (I[1] not > I[0])
2    [1, 1, 1, 1, 1, 1, 1, 1] -> DP[2] = 1 + max([]) (I[2] not > I[0],I[1])
5    [1, 1, 1, 2, 1, 1, 1, 1] -> DP[3] = 1 + max(DP[2]) (I[3] > I[2] and I[3] not > I[0],I[1])
3    [1, 1, 1, 2, 2, 1, 1, 1] -> DP[4] = 1 + max(DP[2]) (I[4] > I[2] and I[4] not > I[0],I[1],I[3])
7    [1, 1, 1, 2, 2, 3, 1, 1] -> DP[5] = 1 + max(DP[1], DP[2], DP[3]) (I[5] > I[1],I[2],I[3] and I[5] not > I[0])
101  [1, 1, 1, 2, 2, 3, 4, 1] -> DP[6] = 1 + max(DP[0], DP[1], DP[2], DP[3], DP[4], DP[5]) (I[6] > all prev numbers)
18   [1, 1, 1, 2, 2, 3, 4, 4] -> DP[7] = 1 + max(DP[0], DP[1], DP[2], DP[3], DP[4], DP[5]) (I[7] > all prev numbers except I[6])
```

```
class Solution:
    def lengthOfLIS(self, nums: List[int]) -> int:
        dp = [1]*len(nums)
        for index in range(1, len(nums)): # skip first element
            dp[index] = 1 + max([dp[j] for j in range(index) if nums[index] > nums[j]], default=0)
        return max(dp, default=0)
```
