# 55. Jump Game

## Dynamic Programming Top Down Solution

- Runtime: O(N^2)
- Space: O(N)
- N = Number of elements in array

You can see that the question is basically asking, given a certain index, can get we to the start or end, depending on which way you look at it.
So in essence, you really only need to save a variable boolean for each index stating that its reachable or not.
This requires an array of size N.
Then its simply iterating each index and setting all reachable indexes to True, requiring two for loops.
This will equate to O(N^2) run-time and O(N) space.

You can do this in a bottom up manner but it will just have to be in reverse.

```
class Solution:
    def canJump(self, nums: List[int]) -> bool:
        if len(nums) == 0:
            return False
        dp = [False] * len(nums)
        dp[0] = True
        for start_index in range(0, len(nums)):
            if dp[start_index]:
                for jump_index in range(start_index, min(len(nums), start_index+nums[start_index]+1)):
                    dp[jump_index] = True
            else:
                break
        return dp[-1]
```

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
