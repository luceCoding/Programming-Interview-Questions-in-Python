# 300. Longest Increasing Subsequence

## Dynamic Programming Solution
- Run-time: O(N^2)
- Space: O(N)
- N = Number of elements in array

If we were to start from the left to the right, we would have seen the longest subsequence on the left side as we are going to the right.
In-order for us to know what those longest subsequences were, we will need a way to store that, hello dynamic programming.

For each element, we would need look at the numbers on the left that are less than the current element.
Now that we know which numbers those are, we can then look at their corresponding longest subsequence in the dynamic programming array.
Therefore, dp[i] = max(for previous dp[j] elements which num[i] < num[j]).

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

## Dynamic Programming with Binary Search Solution
- Run-time: O(Nlog(N))
- Space: O(N)
- N = Number of elements in array

Slightly modifying the previous approach.
We can use a dynamic programming 1d array of 0s but instead store the subsequence themselves instead of what the previous longest subsequence were.
The 1d array can be thought of as keeping a sorted array within itself, there will be a subset of indexes that would represent the longest subsequence.

With this sorted array, we can then binary search it to find where a given n should be placed.
We want to find a number that is greater than or equal to n, then replace that number in the array.
If a number isn't found, we can increase the range of indexes in the sorted array by 1 and add the new n at the end.
By increasing the sorted array by 1, it shows that there is a longer subsequence.
This won't exactly tell us the 'actual' subsequence because of the replacement and ordering but works since the question only cares about what is the longest subsequence.

```
Input: [1,3,6,7,9,4,10,5,6]

[0, 0, 0, 0, 0, 0, 0, 0, 0]
[1, 0, 0, 0, 0, 0, 0, 0, 0]
[1, 3, 0, 0, 0, 0, 0, 0, 0]
[1, 3, 6, 0, 0, 0, 0, 0, 0]
[1, 3, 6, 7, 0, 0, 0, 0, 0]
[1, 3, 6, 7, 9, 0, 0, 0, 0]
[1, 3, 4, 7, 9, 0, 0, 0, 0]
[1, 3, 4, 7, 9, 10, 0, 0, 0]
[1, 3, 4, 5, 9, 10, 0, 0, 0]
[1, 3, 4, 5, 6, 10, 0, 0, 0]
```

```
class Solution(object):
    def lengthOfLIS(self, nums):

        def binary_search_insertion_idx(n):
            left, right = 0, end_idx
            insert_idx = -1
            while left <= right:
                mid_idx = left + ((right - left) // 2)
                if memo[mid_idx] >= n:
                    insert_idx = mid_idx
                    right = mid_idx - 1 # go left
                else: # go right
                    left = mid_idx + 1
            return insert_idx

        memo = [0] * len(nums)
        end_idx = -1
        for n in nums:
            idx = binary_search_insertion_idx(n)
            if idx == -1: # no number found greater than n
                end_idx += 1
                memo[end_idx] = n
            else: # found a number that is greater than n
                memo[idx] = n
        return end_idx + 1
```
