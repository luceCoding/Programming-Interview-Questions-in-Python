# 144. Binary Tree Preorder Traversal

## Recursive Solution
- Runtime: O(N)
- Space: O(H)
- N = Number of elements in tree
- H = Height of tree

Preorder traversal is node -> left -> right

```
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        def preorder_traversal_helper(root, result):
            if root is None:
                return
            result.append(root.val)
            preorder_traversal_helper(root.left, result)
            preorder_traversal_helper(root.right, result)
            
        result = list()
        preorder_traversal_helper(root, result)
        return result
```

## Iterative Solution
- Runtime: O(N)
- Space: O(H)
- N = Number of elements in tree
- H = Height of tree

This is one of the more easier iterative solutions out of the three traversals to understand.
The current first in last out attributes of the stack conforms to the preorder traversal naturally.

```
class Solution:
    def preorderTraversal(self, root: TreeNode) -> List[int]:
        stack = list([root])
        result = list()
        while len(stack) > 0:
            node = stack.pop()
            if node is not None:
                stack.append(node.right)
                stack.append(node.left)
                result.append(node.val)
        return result
```
