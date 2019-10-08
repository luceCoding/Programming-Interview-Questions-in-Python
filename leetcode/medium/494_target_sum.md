# 494. Target Sum

## Recursive Brute Force
- Run-time: 2^N
- Space: 2^N
- N = Number of elements in array

Fairly straight forward, however, will run into time limit exceeded.
Noticed how the run-time is not big O of 2^N, its because this brute force will always run exactly 2^N times.

```
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        
        def sum_helper(curr_sum, idx):
            if curr_sum == S and idx >= len(nums):
                return 1
            if idx >= len(nums):
                return 0
            return sum_helper(curr_sum+nums[idx], idx+1) + sum_helper(curr_sum-nums[idx], idx+1)
        
        return sum_helper(0, 0)
```

## Iterative Solution with Map
- Run-time: O(2^N)
- Space: O(2^N)
- N = Number of elements in array

We can use a dictionary to keep track of the sums and how many paths there are for each sum.
We just need to maintain a rolling dictionary as we traverse across the numbers.
Each traversal we will create new sums and add them into a new dictionary.
We will move the values across from the old dictionary as well.

The dictionary is used to exploit the fact that there can be overlapping sums.
You can imagine the dictionary used for each height/level of the recursion tree, gathering all the sums from the previous summation and reusing it to recalcuate for the current height.

```
                Sums for each height, Key: sum, Val: n_paths
       1        {1: 1, -1: 1}
    +/  \-      
    1     1     {2: 1, 0: 2, -2: 1}
  +/ \- +/ \-
  1  1  1   1   {3: 1, 1: 3, -1: 3, -3: 1}
```

You may think to yourself that the run-time hasn't changed.
You are correct, if given a set of numbers that would create unique sums for each height of the tree, this would end up being O(2^N).
However, since this question is done with addition and subtract, it is more likely there will be overlapping sums.
So the run-time is actually less than O(2^N) for the average case.

```
from collections import defaultdict

class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        if len(nums) == 0:
            return 0
        sum_to_n_paths = defaultdict(int)
        sum_to_n_paths[nums[0]] += 1
        sum_to_n_paths[-nums[0]] += 1
        for n in nums[1:]:
            new_sum_map = defaultdict(int)
            for key, val in sum_to_n_paths.items(): # carry over paths
                new_sum_map[key+n] += val
                new_sum_map[key-n] += val
            sum_to_n_paths = new_sum_map
        return sum_to_n_paths[S]
```
