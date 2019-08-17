# 84. Largest Rectangle in Histogram

## Two Stack Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of heights

This is probably one of the most diffcult questions. 
There are many misleading information for this question out there.
This is by far the most elegant solution I could come up with.

It is easier to first figure out how the solution works with a stack using an increasing and decreasing input, [1,2,3,4] and [4,3,2,1].

For [1,2,3,4], its fairly straight forward, we can keep pushing onto the stack as we get increasing values.
Then when we reach the end, we pop and calculate the width and height at the end.

Before we move further, its important to understand how we will calculate the width. If we instead use the stack to keep a list of indexes, with a for loop, we can use the current index we are at and the index on top of the stack to find the width we should be using.

For [4,3,2,1], you will notice that if we get a decreasing value, its a reason to calculate an area now because we don't know if it will be the max area. However, the tricky part is to know where the start of the width should be. Since the pattern you should now be seeing is to keep an increasing value in the stack, we can deduct that the top of the stack will always represent the index of the start of the width. This is the main part many people get tripped on. 

When you get deeper into the woods of the problem, using [4,3,2,1], you will notice that starting with 4, you can't get a starting width because the stack is currently empty. The trick is to add a placeholder index -1 to represent the beginning. This isn't the only option, instead you can use a condition to check if the stack is empty and keep a counter of the number of indexes you have seen so far instead. But I perfer this way as it removes a lot of extra code.

Additionally, you may have noticed, that your first draft of the problem may contain two while loops, with [1,2,3,4] you haven't calculated anything because nothing was decreasing. To force this, you can add a zero to the end of the input, this will make your first while loop at least run once.

Futhermore, other examples to test your theory are valleys and peak inputs, [1,2,3,2,1] or [3,2,1,0,1,2,3].

So the points to remember are:
- We push increasing values onto the stack.
- If we find a decreasing value, pop onto the stack until we find a value on top of the stack less than the current value. During this, we keep a max area variable.
- We can deduct that, whatever is on top of the stack, is the beginning index for the width.

```
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        heights.append(0)
        height_stack = list([-1])
        ans = 0
        for index, height in enumerate(heights):
            while height < heights[height_stack[-1]]:
                h = heights[height_stack.pop()]
                w = index - height_stack[-1] - 1
                ans = max(ans, h * w)
            height_stack.append(index)
        return ans
```
