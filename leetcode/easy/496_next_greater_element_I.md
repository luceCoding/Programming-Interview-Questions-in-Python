# 496. Next Greater Element I

## Stack Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in both lists

This is an example of where a monotonic stack is useful.
Monotonic stacks only contain elements that are increasing or decresing in value.

Therefore, in this case, we can have a stack that only has increasing values.
For each number in nums2, if we encounter an element larger than the one on top of the stack, we can pop off the top of the stack and map the popped element with the current number as the next largest element.
We continue popping and mapping until the stack is empty, then we can add the current number into the stack.
At the end, we will have a mapping for each number in num2 with their next greater element using this method.

```
from collections import defaultdict

class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        
        def get_greater_map():
            stack = list()
            greater_map = defaultdict(lambda:-1)
            for num in nums2:
                while len(stack) and num > stack[-1]:
                    greater_map[stack.pop()] = num
                stack.append(num)
            return greater_map
                
        greater_map = get_greater_map()
        result = list()
        for num in nums1:
            result.append(greater_map[num])
        return result
```
