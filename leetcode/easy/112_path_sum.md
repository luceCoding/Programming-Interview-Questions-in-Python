# 112. Path Sum

## Recursive Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of nodes in tree

Keeping track of the sum as we go down the tree is one way. 
So that when we reach a leaf node (no children), we can check the current sum against the target sum.
Since its recursion, the current sum before the recursion call would be reused if we hit a dead end, so we don't need to do any subtraction from the current sum, we let recursion do that for us.

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

## Iterative Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of nodes in tree

Similar to the recursive solution, but we need to store an additional current sum with the node in the stack.
Since we will never need to go back up the tree once we have traverse down, there is no need to implement a global sum.

```
class Solution:
    def hasPathSum(self, root: TreeNode, target: int) -> bool:
        stack = list([(root, 0)])
        while len(stack) > 0:
            curr_node, curr_sum = stack.pop()
            if curr_node is None:
                continue
            curr_sum += curr_node.val
            if curr_node.left is None and curr_node.right is None:
                if curr_sum == target:
                    return True
                continue
            stack.append((curr_node.right, curr_sum))
            stack.append((curr_node.left, curr_sum))
        return False
```
