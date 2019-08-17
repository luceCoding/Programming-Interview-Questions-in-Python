# 85. Maximal Rectangle

## Stack Solution
- Runtime: O(R*C)
- Space: O(C)
- R = Number of rows
- C = Number of columns

Most of the heavy lifting is done by the algorithm of **#84 Largest Rectangle in Histogram**.
It is best to do question #84 before doing this one.

If we keep a running sum of heights going from top to the bottom row, we will mimic the **question #84**.
Then its just a need of reusing the algorithm to find the max area for that current row.

```
class Solution:
    def maximalRectangle(self, matrix: List[List[str]]) -> int:
        running_heights = [0] * (len(matrix[0]) if len(matrix) != 0 else 0)
        max_area = 0
        for row in matrix:
            for index, element in enumerate(row):
                if element == '0':
                    running_heights[index] = 0
                else:
                    running_heights[index] += 1
            max_area = max(max_area, self.calc_max_area(running_heights))
        return max_area
            
    def calc_max_area(self, heights):
        stack = list([-1])
        heights.append(0)
        max_area = 0
        for index, height in enumerate(heights):
            while height < heights[stack[-1]]:
                h = heights[stack.pop()]
                w = index - stack[-1] - 1
                max_area = max(max_area, h*w)
            stack.append(index)
        return max_area
```
