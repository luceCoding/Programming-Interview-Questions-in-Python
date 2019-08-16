# 84. Largest Rectangle in Histogram

## Two Stack Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of heights

```
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        pos_stack, height_stack = list(), list()
        max_area = last_p = 0
        for index, height in enumerate(heights):
            if len(height_stack) == 0 or height > height_stack[-1]:
                height_stack.append(height)
                pos_stack.append(index)
            elif height < height_stack[-1]:
                while len(height_stack) != 0 and height < height_stack[-1]:
                    last_p = pos_stack[-1]
                    max_area = max(max_area, self.calc_area(index, \
                                                            pos_stack, \
                                                            height_stack))
                height_stack.append(height)
                pos_stack.append(last_p)
        while len(height_stack) != 0:
            max_area = max(max_area, self.calc_area(len(heights), \
                                                    pos_stack, \
                                                    height_stack))
        return max_area
                
    def calc_area(self, curr_position, pos_stack, height_stack):
        return height_stack.pop() * (curr_position - pos_stack.pop())
```
