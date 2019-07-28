# 494. Target Sum

## Recursive Brute Force
- Runtime: 2^N
- Space: 2^N
- N = Number of elements in array

Fairly straight forward, however, will run into time limit exceeded.
Noticed how the run-time is not big O of 2^N, its because this brute force will always run exactly 2^N times.

```
class Solution:
    def findTargetSumWays(self, nums: List[int], S: int) -> int:
        
        def find_sum_ways_helper(nums, curr_sum, start_i):
            if curr_sum == S and start_i >= len(nums):
                return 1
            elif start_i >= len(nums):
                return 0
            n_sums = 0
            n_sums += find_sum_ways_helper(nums, curr_sum + nums[start_i], start_i+1)
            n_sums += find_sum_ways_helper(nums, curr_sum - nums[start_i], start_i+1)
            return n_sums
        
        return find_sum_ways_helper(nums, 0, 0)
```

## Iterative Solution with Map
- Runtime: O(N^2)
- Space: O(N^2)
- N = Number of elements in array

We can use a dictionary to keep track of the sums and how many paths there are for each sum.
We just need to maintain a rolling dictionary as we traverse across the numbers.
Each traversal we will create new sums and add them into a new dictionary.
We will move the values across from the old dictionary as well.

For the run-time, you may think that the run-time hasn't changed.
Why is this an improvement?
An input like [1,10,100,1000,10000...] will achieve N^2 run time.
However, given any other input, since its add and subtract and not multiply or divide, its unlikely and its more likely we will have overlapping sums.
So the run time is actually less than O(2^N) on most cases while the brute force solution above will always be ran at 2^N.

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
