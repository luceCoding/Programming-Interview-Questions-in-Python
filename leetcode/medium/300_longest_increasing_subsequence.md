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

## Binary Search Solution
- Run-time: O(Nlog(N))
- Space: O(N)
- N = Number of elements in array

If we instead used a sorted array to keep a subsequence, we can decide whether or not we can create a larger subsequence with binary search.
For each n, we can binary search the sorted array for the left-most number that is larger or equal to n.
If that number is found, we can replace that number with n.
If no number is found, we can append n to the end of the array, this basically means we can create a larger subsequence.

Since we replace numbers in the sorted array, the sorted array does not represent the 'actual' longest subsequence.
We need to replace numbers in the array because we can potentially create a larger subsequence.
For example, given an input of [1,2,3,99,4,5,6], when we reach number 5, if we didn't replace 99 in the sorted array, we can never increase the subsequence by 1 when the sorted array was [1,2,3,99] instead of [1,2,3,4].

```
Input: [1,3,6,7,9,4,10,5,6]

[]
[1]
[1, 3]
[1, 3, 6]
[1, 3, 6, 7]
[1, 3, 6, 7, 9]
[1, 3, 4, 7, 9]
[1, 3, 4, 7, 9, 10]
[1, 3, 4, 5, 9, 10]
[1, 3, 4, 5, 6, 10]
```

```
class Solution(object):
    def lengthOfLIS(self, nums):

        def binary_search_insertion_idx(n):
            left, right = 0, len(sorted_subseq) - 1 if len(sorted_subseq) != 0 else -1
            insert_idx = -1
            while left <= right:
                mid_idx = left + ((right - left) // 2)
                if sorted_subseq[mid_idx] >= n:
                    insert_idx = mid_idx
                    right = mid_idx - 1 # go left
                else: # go right
                    left = mid_idx + 1
            return insert_idx

        sorted_subseq = list()
        for n in nums:
            idx = binary_search_insertion_idx(n)
            if idx == -1: # no number found greater than n
                sorted_subseq.append(n)
            else: # found a number that is greater than n
                sorted_subseq[idx] = n
        return len(sorted_subseq)
```
