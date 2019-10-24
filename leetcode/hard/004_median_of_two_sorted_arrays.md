#4. Median of Two Sorted Arrays

## Linear Solution
- Run-time: O(N)
- Space: O(1)
- N = Number of elements in both lists

For the first solution, lets dissect what it means to find a median in two sorted lists.

Some properties we know are:
- Total length of both lists.
- Both lists are sorted.

If you had one list, it would be as easy as index = total length // 2 and list[index] as the median if even, else list[index+1] if odd.
Since we know the total length, we can still find how many times we need to traverse until we hit a median.
With one list, we can treat the list as having a left array and a right array.
Knowing how big the left array would be as one list and the fact that both lists are sorted, we can traverse the left side of both lists until we have traversed enough indexes.
When the left most index of both lists are found, the left index + 1 of both lists would be part of the right array.

```
class Solution:
    def findMedianSortedArrays(self, nums1: List[int], nums2: List[int]) -> float:

        def get_right_most_indexes_of_array1():
            n_moves_to_med = (len(nums1) + len(nums2)) // 2
            left1_idx = left2_idx = -1  
            while n_moves_to_med and left1_idx+1 < len(nums1) and left2_idx+1 < len(nums2):
                n_moves_to_med -= 1
                if nums1[left1_idx+1] < nums2[left2_idx+1]:
                    left1_idx += 1
                else:
                    left2_idx += 1
            while n_moves_to_med and left1_idx+1 < len(nums1):
                n_moves_to_med -= 1
                left1_idx += 1
            while n_moves_to_med and left2_idx+1 < len(nums2):
                n_moves_to_med -= 1
                left2_idx += 1
            return (left1_idx, left2_idx)

        left1_idx, left2_idx = get_right_most_indexes_of_array1()
        # left most number of array2
        n2 = min(nums1[left1_idx+1] if 0 <= left1_idx+1 < len(nums1) else float('inf'),
                 nums2[left2_idx+1] if 0 <= left2_idx+1 < len(nums2) else float('inf'))
        if (len(nums1) + len(nums2)) % 2 == 0: # is even?
            # right most number of array1
            n1 = max(nums1[left1_idx] if 0 <= left1_idx < len(nums1) else float('-inf'),
                     nums2[left2_idx] if 0 <= left2_idx < len(nums2) else float('-inf'))
            return (n1 + n2) / 2
        return n2 # is odd
```
