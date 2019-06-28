# 33. Search in Rotated Sorted Array

## From scratch iterative solution
- Runtime: O(log(N))
- Space: O(1)
- N = Number of elements in array

Take the example, "5,6,1,2,3,4" and "3,4,5,6,1,2" and "1,2,3,4,5,6". 
If you had a sorted array, it would be easy to just binary search that. But if its rotated, you can still binary search the array but only parts of it. What is stopping you from immediately binary searching the array is that you don't know where the rotation point is. Once you can find it then the solution becomes easier as you can just binary search the sub-array containing your target. You can find the beginning index of the rotated sub-array by binary searching it.

```
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        def get_first_sorted_index(nums):
            left_i, right_i = 0, len(nums)-1
            while left_i <= right_i:
                mid_i = int(abs(left_i + (right_i - left_i)/2))
                if mid_i != 0 and nums[mid_i-1] > nums[mid_i]:
                    return mid_i
                if nums[mid_i] < nums[left_i]: # go left
                    right_i = mid_i-1
                elif nums[mid_i] > nums[right_i]: # go right
                    left_i = mid_i+1
                else: # sub-array is sorted, keep going left
                    right_i = mid_i-1
            return 0
            
        def binary_search_from_range(start, end, target, nums):
            while start <= end:
                mid_i = int(abs(start + (end - start)/2))
                if nums[mid_i] == target:
                    return mid_i
                if nums[mid_i] < target: # go right
                    start = mid_i+1
                elif nums[mid_i] > target: # go left
                    end = mid_i-1
            return -1
            
        if len(nums) == 0:
            return -1
        first_sorted_index = get_first_sorted_index(nums)
        if target <= nums[-1]: # right side
            return binary_search_from_range(first_sorted_index, len(nums)-1, target, nums)
        # left side
        return binary_search_from_range(0, first_sorted_index, target, nums)
```
