# 11. Container With Most Water

## Two pointer one pass solution

- Runtime: O(N)
- Space: O(1)
- N = Number of elements in list

The intution can be gained by thinking about how to carry the most water in an increasing and decreasing input like [1,2,3,4] or [4,3,2,1].
If you begin with the left and right, you can figure out how much water you can have with just those two inputs.

The next step is to figure out which direction you should move inwards, either the left or the right, squeezing into the middle.
You can figure that out by using peak and valley inputs, like [1,2,3,2,1] or [3,2,1,2,3].
What you begin to notice is that, the only way we can increase the amount of water is by moving the lowest height and finding a larger height.
If both left and right heights are the same, we can pick an arbitrary side to move.

```
class Solution:
    def maxArea(self, height: List[int]) -> int:
        left, right = 0, len(height)-1
        max_area = 0
        lowest_height = min(height[left], height[right])
        while left < right:
            lowest_height = min(height[left], height[right])
            length = right - left
            max_area = max(max_area, lowest_height * length)
            if height[left] <= height[right]:
                left += 1
            else:
                right -= 1
        return max_area
```
