# 113. Path Sum II

## Recursive Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of nodes in tree

There are a few things that need to be added outside of a normal traversal of a binary tree.
Outside of these things, everything else is a post order traversal.

1. Keep track of the sum as we traverse.
2. Have a result we can modify as we traverse.
3. Keep track of what nodes we have already traversed for when we find a matching sum.

```
class Solution:
    def pathSum(self, root: TreeNode, target: int) -> List[List[int]]:
        def path_sum_helper(root, target, curr_sum, result, stack):
            if root is None:
                return
            stack.append(root.val)
            curr_sum += root.val
            if root.left is None and root.right is None and target == curr_sum:
                result.append([s for s in stack])
            path_sum_helper(root.left, target, curr_sum, result, stack)
            path_sum_helper(root.right, target, curr_sum, result, stack)
            stack.pop()
            
        result = list()
        path_sum_helper(root, target, 0, result, [])
        return result
```
