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

## Iterative Solution (Lazy Approach)
- Runtime: O(N)
- Space: O(N)
- N = Number of nodes in tree

Similar to the recursive solution, but we need to store an additional current sum with the node in the stack.
By having both the current node and current sum at any given point, its simpliy a matter of visiting all nodes in the tree, then calculating the result.

I would consider this implementation "lazy", as it doesn't truely show full understanding of stacks and recursion.
If you follow the traversal of this implementation, it doesn't follow any of the pre-order, in-order, or post-order traversals, since it total skips nodes and doesn't backtrack up at all.

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


## Inorder Iterative Solution (Preferred Approach)

This implementation is similar to the above approach, however, it falls in line with how you would do an inorder traversal iteratively due to backtracking. It is important to fully understand this implementation. Try drawing it out if it confuses you, it will click better. Then try it doing pre-order and post-order traversals.

```
class Solution:
    def hasPathSum(self, root: TreeNode, target: int) -> bool:
        stack = list()
        curr_node, curr_sum = root, 0
        while True:
            if curr_node is not None: # going down the tree
                curr_sum += curr_node.val
                stack.append((curr_node, curr_sum))
                if curr_node.left is None \
                  and curr_node.right is None \
                  and curr_sum == target:
                    return True
                curr_node = curr_node.left
            elif len(stack) > 0: # going up the tree
                curr_node, curr_sum = stack.pop()
                curr_node = curr_node.right
            else:
                break
        return False
```
