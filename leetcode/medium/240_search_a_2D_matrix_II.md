# 240. Search a 2D Matrix II

## Seudo-Binary Search

- Runtime: O(Rlog(C)) or O(Clog(R))
- Space: O(1)
- R = Number of rows
- C = Number of columns

If we binary search each row, we can find if a number exists on that row fairly easy.
However, the worst case is that we have to binary search every row for the target.
You can also binary search in the opposite direction by each column, will end up with the same solution.

```
class Solution:
    def searchMatrix(self, matrix, target):
        
        def binary_search(nums):
            left = 0
            right = len(nums)-1
            last_index = -1
            while left <= right:
                mid = left + (right-left // 2)
                if nums[mid] == target:
                    return mid
                elif nums[mid] < target: # go right
                    last_index = mid
                    left = mid+1
                else: # go left
                    right = mid-1
            return last_index
        
        if len(matrix) == 0 or len(matrix[0]) == 0:
            return False
        for row in matrix:
            if row[0] <= target <= row[-1]:
                col_idx = binary_search(row)
                if row[col_idx] == target:
                    return True
        return False
```

## Best Solution

- Runtime: O(R+C)
- Space: O(1)
- R = Number of rows
- C = Number of columns

The previous solution partially used the properties of the sorted matrix, but not at the fullest extend.
Depending on where your starting point is on the matrix, for this example, the top-right most element in the matrix.
We can ask, does this element exist on this row?
We will check if the current element is greater than or equal to the target.
If yes, we move to the left, if not, we move down one row.
We repeat this until we have exhausted possible numbers.

This method works because of the relationship that the current element shows whether the bottom portion is worth searching.
Couple that with whether the current row is worth searching once we reach a number that is larger than the target.
These two cases will eliminate our search space over this sorted matrix.

```
class Solution:
    def searchMatrix(self, matrix, target):
        if not any(matrix):
            return False
        row_idx, col_idx = 0, len(matrix[0])-1
        # start at top-right most element
        while row_idx < len(matrix) and col_idx >= 0:
            if matrix[row_idx][col_idx] == target:
                return True
            elif matrix[row_idx][col_idx] > target: # go left
                col_idx -= 1
            elif matrix[row_idx][col_idx] < target: # go down
                row_idx += 1
        return False
```
