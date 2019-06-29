# 581. Shortest Unsorted Continuous Subarray

## Best solution
- Runtime: O(N)
- Space: O(1)
- N = Number of elements in array

The idea is quite simple. Start from left to right, find the first unsorted index, then from right to left, find the last unsorted index.
From then, you have three subarrays, the left sub-array, the middle sub-array, and the right sub-array. 
The middle sub-array is obviously your known set of unsorted numbers but you do not know if your middle sub-array can be further extended to the left or the right.
For example, [1,2,3,4,1,2,1,2,3], the first pass through will only identify index 3 and index 6 as the middle sub-array. 
However, you need a second pass through to identify index 1 to index 8 by checking if the numbers of the left or right sub-arrays are in the middle sub-array.

```
class Solution:
    def findUnsortedSubarray(self, nums: List[int]) -> int:
        first = last = None
        for index, n in enumerate(nums[:-1]):
            if n > nums[index+1]:
                first = index
                last = index+1
                break
        for index, n in reversed(list(enumerate(nums))):
            if index > 0 and n < nums[index-1]:
                last = index
                break
        if first is not None:
            MIN, MAX = min(nums[first:last+1]), max(nums[first:last+1])
            # check outer sub-arrays for similar numbers
            for index, n in enumerate(nums[:first]):
                if n in range(MIN+1, MAX+1):
                    first = index
                    break
            for index, n in reversed(list(enumerate(nums))):
                if index <= last: # don't iterate into known unsorted array
                    break
                if n in range(MIN, MAX):
                    last = index
                    break
            return last-first+1
        return 0
```
