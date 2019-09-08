# 560. Subarray Sum Equals K

## Brute-Force Solution

- Runtime: O(N^2)
- Space: O(1)
- N = Number of elements in array

Simple iteration on all possible combinations of sub-arrays.

```
class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        n_subarrays = 0
        for start in range(len(nums)):
            rolling_sum = 0
            for end in range(start, len(nums)):
                rolling_sum += nums[end]
                if rolling_sum == k:
                    n_subarrays += 1
        return n_subarrays
```

## Dictionary Solution

- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

To visialize this solution, let's take this example.
The dashes represent a solution set, result = 3.
Let's look at one of the solution set.

```
k=1
[-1,1,-1,1]
    ------  <-- k
 ---------  <-- sum
 ---        <-- x
 
x = sum - k
```

To find X, we need to take the current rolling sum and subtract it with k.
So this means, if we use a hash map (Key: sum, Value: occurance), iterate from the beginning to the end and store the current sums into the hash map.
We can then check if X exists in our hash map to see that we can make a sub-array.

```
from collections import defaultdict

class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        sum_map = defaultdict(int)
        rolling_sum = n_subarrays = 0
        sum_map[0] = 1
        for n in nums:
            rolling_sum += n
            if sum_map[rolling_sum-k] != 0:
                n_subarrays += sum_map[rolling_sum-k]
            sum_map[rolling_sum] += 1
        return n_subarrays
```
