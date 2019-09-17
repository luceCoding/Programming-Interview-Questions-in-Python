# 912. Sort an Array

## Quick Sort

- Runtime: O(Nlog(N)), worst case O(N^2)
- Space: O(log(N))
- N = Number of elements in list

It is important to know the inner workings of a quicksort and how to implement it.

Quick sort works by selecting a pivot, usually the last index of the array. 
Then checking each number in the partition N times to the pivot.
We will need to keep a separate index to where the last unsorted number is in the array. 
If we encounter a number that is less than or equal to the pivot, we would swap it with the last unsorted number and increment the last unsorted index.
Lastly, swap the pivot to the last unsorted index, this represents the only truely sorted number in the entire array.
The pivot is now in the correct place.
We then partition the array in two halves and call a recursion on each half, they will be figuring out where the correct placement of the next pivot should be.
This is where the log(N) comes into play, since each time we call the recursion, the array is half of what it use to be.

If the pivot is always selected to be the smallest number of the array, we can have a worst case scenario of O(N^2).
Hence, never creating two partitions.

```
class Solution(object):
    def sortArray(self, nums):
        
        def quick_sort(nums):
            if len(nums) == 0:
                return []
            pivot = nums[-1]
            last_unsorted_idx = 0
            for idx, n in enumerate(nums[:-1]):
                if n <= pivot:
                    nums[last_unsorted_idx], nums[idx] = nums[idx], nums[last_unsorted_idx]
                    last_unsorted_idx += 1
            nums[last_unsorted_idx], nums[-1] = nums[-1], nums[last_unsorted_idx]
            left_sorted = quick_sort(nums[:last_unsorted_idx])
            right_sorted = quick_sort(nums[last_unsorted_idx+1:])
            return left_sorted + [nums[last_unsorted_idx]] + right_sorted
        
        return quick_sort(nums)
```
