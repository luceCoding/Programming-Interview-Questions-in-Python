# 145. Binary Tree Postorder Traversal

## Recursive Solution
- Runtime: O(N)
- Space: O(H)
- N = Number of elements in tree
- H = Height of tree

Post order is left, right, node.

The recusive solution is fairly easy. Most of the heavy lifting is abstracted away by the recursion call.

```
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        def postorder_traversal_helper(root, result):
            if root is None:
                return
            postorder_traversal_helper(root.left, result)
            postorder_traversal_helper(root.right, result)
            result.append(root.val)
            
        result = list()
        postorder_traversal_helper(root, result)
        return result
```

## Iterative Solution
- Runtime: O(N)
- Space: O(H)
- N = Number of elements in tree
- H = Height of tree

The iterative solution for post order is fairly diffucult to come up with on your own.
It requires two stacks. 
The first stack is used to traverse the tree but in the opposite direction (node -> right -> left).
During the traversal, the 1st stack will transfer its nodes to the 2nd stack, this will place the nodes in the reverse order or post-order (left -> right -> node) when they are popped off the stack later.
I recommend drawing this out, as its important to understand the relationships and responsibilities.

```
class Solution:
    def postorderTraversal(self, root: TreeNode) -> List[int]:
        if root is None:
            return []
        stack1, stack2 = list([root]), list()
        result = list()
        while len(stack1) > 0:
            node = stack1.pop()
            stack2.append(node)
            if node.left is not None:
                stack1.append(node.left)
            if node.right is not None:
                stack1.append(node.right)
        while len(stack2) > 0:
            node = stack2.pop()
            result.append(node.val) # <-- Business logic goes here
        return result
```
