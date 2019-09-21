# 75. Sort Colors

## Three pointer one pass solution

- Runtime: O(N)
- Space: O(1)
- N = Number of elements in list

Since we know the range of numbers, 0, 1 and 2, we can just use two pointers for each end of the array.
The left pointer will point to the first unsorted number != 0 and the right pointer will point to the last unsorted number != 2.
We will then traverse the array from left to right swapping the values with a third pointer to either the left or right depending if its a 0 or 2, ignoring 1s, then incrementing the left or right pointer.
Eventually, when we reach the right pointer, it would have sorted the entire array and all the 1s will be in the middle.

```
class Solution:
    def sortColors(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        right = len(nums)-1
        curr_index = left = 0
        while curr_index <= right:
            if nums[curr_index] == 0:
                nums[curr_index], nums[left] = nums[left], nums[curr_index]
                left += 1
                if left > curr_index:
                    curr_index = left
            elif nums[curr_index] == 2:
                nums[curr_index], nums[right] = nums[right], nums[curr_index]
                right -= 1
            else:
                curr_index += 1
```
