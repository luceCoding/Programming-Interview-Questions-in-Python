# 55. Jump Game

## Best Solution
- Runtime: O(N)
- Space: O(1)
- N = Number of elements in array

You can think of this solution by imagining a car driving down the road, each element in the array represents a gas station.
However, each gas station only allows a refill up to a certain max number.
So if you have 4 gallons of gas in your car and the gas station allows up to 2, you can't refill here.
On the other hand, if you have 2 gallons of gas and the gas station allows up to 3, you take the refill, now your car is set to 3 gallons of gas.
Then its simply driving as far as possible until you run out of gas.

This is a much simplier approach to this question that is not recommended in the solutions of leetcode. 
Instead of reversing or backtracking, we just move left to right.
This is still a greedy approach.

```
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        if len(nums) == 0:
            return False
        n_jumps, curr_index = 1, 0
        while n_jumps != 0:
            if curr_index == len(nums)-1:
                return True
            n_jumps -= 1
            n_jumps = max(nums[curr_index], n_jumps)
            curr_index += 1
        return False
```
