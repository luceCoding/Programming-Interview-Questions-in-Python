# 112. Path Sum

## Recursive Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of nodes in tree

Keeping track of the sum as we go down the tree is one way. 
So that when we reach a leaf node (no children), we can check the current sum against the target sum.
Since its recursion, the current sum before the recursion call would be used so we don't need to subtract from the current sum.
We let recursion do that for us.

Since we will be visiting all the nodes, the runtime is O(N). 
In the worst case, we would have to have every node on the stack if the whole tree was just a linked list, space would be O(N).

```
class Solution:
    def hasPathSum(self, root: TreeNode, sum: int) -> bool:
        def has_path_sum_helper(root, curr_sum, target):
            if root is None:
                return False
            curr_sum += root.val
            if root.left is None and root.right is None:
                return curr_sum == target
            return has_path_sum_helper(root.left, curr_sum, target) \
                or has_path_sum_helper(root.right, curr_sum, target)
        
        return has_path_sum_helper(root, 0, sum)
```
