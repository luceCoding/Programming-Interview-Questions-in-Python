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

## Quick Sort In-Place

- Runtime: O(Nlog(N)), worst case O(N^2)
- Space: O(log(N))
- N = Number of elements in list

To do an in-place quicksort with python, we have to avoid using string slicing.

Note: There is an unstable version of in-place quick sort that has an O(1) space complexity.

```
class Solution(object):
    def sortArray(self, nums):
        
        def quick_sort(l_idx, r_idx):
            if l_idx >= r_idx:
                return
            pivot = nums[r_idx]
            last_unsorted_idx = l_idx
            for idx in range(l_idx, r_idx):
                if nums[idx] <= pivot:
                    nums[last_unsorted_idx], nums[idx] = nums[idx], nums[last_unsorted_idx]
                    last_unsorted_idx += 1
            nums[last_unsorted_idx], nums[r_idx] = nums[r_idx], nums[last_unsorted_idx]
            quick_sort(l_idx, last_unsorted_idx-1)
            quick_sort(last_unsorted_idx+1, r_idx)
            
        quick_sort(0, len(nums)-1)
        return nums
```

## Merge Sort

- Runtime: O(Nlog(N))
- Space: O(N)
- N = Number of elements in list

Merge sort first breaks up the list into elements of one.
Then from those small elements, merges them together by comparing each left and right list.
Each left and right list that gets returned is in sorted order, so its a simple two pointer solution to merge the two lists into a larger sorted list. Return this larger sorted list up the recursion and repeat until the entire subset is sorted.

Merge sort is considered a stable sort.

```
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        
        def merge_sort(nums):
            if len(nums) <= 1:
                return nums
            mid_idx = len(nums) // 2
            left = merge_sort(nums[:mid_idx])
            right = merge_sort(nums[mid_idx:])
            left_idx = right_idx = 0
            merged = list()
            while left_idx < len(left) and right_idx < len(right):
                if left[left_idx] < right[right_idx]:
                    merged.append(left[left_idx])
                    left_idx += 1
                else:
                    merged.append(right[right_idx])
                    right_idx += 1
            merged += left[left_idx:]
            merged += right[right_idx:]
            return merged
        
        return merge_sort(nums)
```

## Bubble Sort

- Runtime: O(N^2)
- Space: O(1)
- N = Number of elements in list

Each time we perform a bubble sort, we are essential pushing the max value of the array to the back of the array.

The advantage of bubble sort is the lack of memory or constant memory space.

```
class Solution:
    def sortArray(self, nums: List[int]) -> List[int]:
        for i in reversed(range(0, len(nums))):
            for curr in range(0, i):
                if nums[curr] > nums[curr+1]:
                    nums[curr], nums[curr+1] = nums[curr+1], nums[curr]
        return nums
```
