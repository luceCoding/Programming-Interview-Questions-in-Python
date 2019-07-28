# 34. Find First and Last Position of Element in Sorted Array

## Iterative binary search solution
- Runtime: O(log(N))
- Space: O(1)
- N = Number of elements in array

Knowing how to write a binary search whether iteratively or recursively is a must.
However, there is a second thing being tested, whether you follow DRY (Don't repeat yourself).

```
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        left_most_index = self.get_left_or_right_most_index(nums, target)
        right_most_index = self.get_left_or_right_most_index(nums, target, search_left=False)
        return [left_most_index, right_most_index]
        
    def get_left_or_right_most_index(self, nums, target, search_left=True):
        left, right = 0, len(nums)-1
        result_index = None
        while left <= right:
            mid = left + ((right - left)//2)
            if nums[mid] == target:
                result_index = mid
            if search_left: # get left most index
                if nums[mid] < target: # go right
                    left = mid+1
                else: # go left
                    right = mid-1
            else: # get right most index
                if nums[mid] > target: # go left
                    right = mid-1
                else: # go right
                    left = mid+1
        return result_index if result_index is not None else -1
```
