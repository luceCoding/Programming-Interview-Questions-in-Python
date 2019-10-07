# 416. Partition Equal Subset Sum

## Recursive Solution
- Run-time: O(2^N)
- Space: O(N)
- N = Number of Nums

Splitting an array into equal parts can be rephrased to, find a sub-array that is half of the total sum.
With this we can generate a solution that focuses on finding a combination of numbers that contain such a sum.

To find this target sum, we can use recursion, simply ask two questions, do we want this number or don't want this number?

Lastly, there are some optimizations we can do, if we have an input of [1,1,1,...,1,1,100] that cannot be partitioned.
We would end up repeating recursion calls for each 1, to keep 1 or not.
When we end up not using the number, we would ask the same 2 questions again.
This creates a time limit exceeded issue and we can optimize this by skipping repeated numbers when we don't want to use this number.

```
class Solution:
    def canPartition(self, nums: List[int]) -> bool:

        def does_subset_sum_exist(target, idx=0):
            if target == 0:
                return True
            if target < 0 or idx > len(nums)-1:
                return False
            if does_subset_sum_exist(target-nums[idx], idx+1):
                return True
            while idx+1 < len(nums) and nums[idx] == nums[idx+1]: # skip duplicate elements
                idx += 1
            return does_subset_sum_exist(target, idx+1)

        if len(nums) == 0:
            return True
        num_sum = sum(nums)
        if num_sum % 2: # odd
            return False
        return does_subset_sum_exist(num_sum//2)
```

## Dynamic Programming Solution
- Run-time: O(T * N)
- Space: O(T * N)
- N = Number of Nums
- T = Sum of Nums divided by 2


The sub-problem of finding the subset sum is essentially a 0/1 knapsack problem.
This particular variant of the knapsack problem can be solved using a 2d array of booleans.
To identify that this is a 0/1 is the fact we cannot split the numbers, if we could then a greedy algorithm would work here. If splitting was allowed, we could sort it then take the smallest numbers until we reach the max and split the difference for the result.

We can construct a solution starting at sum 0 to the target sum.
Each column will represent the nums and each row will be the sum.

```
Given [1,2,3,4]
Rows: sums from 0 to target sum
Columns: numbers

Initial DP:
    0  1  2  3  4
0 [[T, F, F, F, F],
1  [F, F, F, F, F],
2  [F, F, F, F, F],
3  [F, F, F, F, F],
4  [F, F, F, F, F],
5  [F, F, F, F, F]]

Final DP:
    0  1  2  3  4
0 [[T, T, T, T, T],
1  [F, T, T, T, T],
2  [F, F, T, T, T],
3  [F, F, T, T, T],
4  [F, F, F, T, T],
5  [F, F, F, T, T]]
```

The intuition is gathered by using the two questions we asked earlier, whether to keep this number or not.

To not keep this number is simply looking at the current sum and the previous number.
That is because if we could not use this number which can be defined that previous number + 0, then the previous number needs to be True to add up to this current sum.

To keep this number, we have to look at the current sum - current number of the previous number.
That would mean some previous sum from nums[:i] must have been True for us to perform sum(nums[:i]) + nums[i] == current sum.

dp\[curr_sum][curr_num] = dp\[curr_sum][prev_num] or dp\[curr_sum-curr_num][prev_num]

```
class Solution:
    def canPartition(self, nums: List[int]) -> bool:

        def does_subset_sum_exist(target):
            dp = [[False] * (len(nums)+1) for _ in range(target+1)]
            dp[0][0] = True
            for curr_sum in range(0, target+1):
                for n_idx, num in enumerate(nums, 1):
                    dp[curr_sum][n_idx] = dp[curr_sum][n_idx-1] or (dp[curr_sum-num][n_idx-1] if curr_sum-num >= 0 else False)
            return dp[-1][-1]

        if len(nums) == 0:
            return True
        num_sum = sum(nums)
        if num_sum % 2: # odd
            return False
        return does_subset_sum_exist(num_sum//2)
```
