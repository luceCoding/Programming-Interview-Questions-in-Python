# 238. Product of Array Except Self

## Dynamic Programming Approach
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

The idea is to calculate the products going from left to right, then again going right to left.
Storing these calculations as two arrays will allow one last calculation to find the products from the left and right for each number.

```
For example:
Input: [1,2,3,4]
Left Array: [1,1,2,6]
Right Array: [24,12,4,1]
Result: [24,12,8,6]
```

```
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        if len(nums) == 0:
            return []
        left, right = self.calc_left_product(nums), self.calc_left_product(nums[::-1])[::-1]
        result = list()
        for l, r in zip(left, right):
            result.append(l*r)
        return result
        
    def calc_left_product(self, nums):
        nums = [1] + nums[:-1]
        result = list(itertools.accumulate(nums, operator.mul))
        return result
```

## Best Solution (Constant Space)
- Runtime: O(N)
- Space: O(1)
- N = Number of elements in array

Similar to the solution above, instead of creating two arrays, we can first create an array for the products to the right of each number.
This array can then be used as the final result, we can then calculate the left products as we modify the first array from left to right.
This is constant space, assuming the returned result array is not considered extra space.

```
class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        if len(nums) == 0:
            return []
        result = self.calc_left_product(nums[::-1])[::-1] # right products
        curr_prod = 1
        for index, n in enumerate(nums):
            result[index] *= curr_prod
            curr_prod *= n
        return result
        
    def calc_left_product(self, nums):
        nums = [1] + nums[:-1]
        result = list(itertools.accumulate(nums, operator.mul))
        return result
```
