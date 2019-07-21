# 94. Binary Tree Inorder Traversal

## Recursive solution
- Runtime: O(N)
- Space: O(1)
- N = Number of elements in tree

Make sure you understand the recursive solution first before attempting the iterative one.
The main point is to understand is the recursive call and how it backtracks to the previous call when it reaches the base case.

```
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        def inorder_traversal_helper(root, result):
            if root is None:
                return
            inorder_traversal_helper(root.left, result)
            result.append(root.val)
            inorder_traversal_helper(root.right, result)
            
        result = list()
        inorder_traversal_helper(root, result)
        return result
```

## Iterative solution
- Runtime: O(N)
- Space: O(1)
- N = Number of elements in tree

To fully understand this implementation, I recommend you draw this out step by step.
Iterative inorder traversal can be confusing, no amount of words can help describe its inner workings.
Don't try to memorize this, understand it.

A good starting point for converting a recursive binary tree solution into an iterative one is to start with a stack, current node variable and a while loop.
Those should be your starting ingredients.

```
class Solution:
    def inorderTraversal(self, root: TreeNode) -> List[int]:
        stack, result = list(), list()
        curr_node = root
        while True:
            if curr_node is not None: # Going down the tree
                stack.append(curr_node)
                curr_node = curr_node.left
            elif len(stack) > 0: # Going up the tree (backtracking)
                curr_node = stack.pop()
                result.append(curr_node.val)
                curr_node = curr_node.right
            else:
                break
        return result
```
