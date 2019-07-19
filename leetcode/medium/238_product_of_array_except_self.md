# 238. Product of Array Except Self

## Dynamic Programming Approach
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

The idea is to calculate the products going from left to right, then again going right to left.
Storing these calculations as two arrays will allow one last calculation to find the products from the left and right for each number.

```
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        if len(nums) == 0:
            return []
        left, right = self.calc_left_product(nums), self.calc_left_product(nums[::-1])[::-1]
        result = [1]*len(nums)
        for index, n in enumerate(zip(left, right)):
            result[index] = n[0]*n[1]
        return result
        
    def calc_left_product(self, nums):
        result = [1]*len(nums)
        curr_prod = nums[0]
        for index, n in enumerate(nums[1:], 1):
            result[index] = curr_prod
            curr_prod *= n
        return result
```
