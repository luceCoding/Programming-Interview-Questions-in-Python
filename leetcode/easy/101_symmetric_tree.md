# 101. Symmetric Tree

## Recursive Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of nodes in tree

I recommend drawing out a tree of height 4 to find the intuition for the solution.
You should notice that everytime you want to go left, you care about the one on the most right of the tree.
The tough part is what to do with the inner nodes?

If your recursion passed in two nodes at a time, a left node and a right node, and traverse down the tree one height at a time. 
You can then call the recursion twice, one of the recursions will look at left and right while the other will go right then left.
This will allow you to keep nodes that are across the span of the tree.

You can also think of this by dividing the tree in two parts, say a left tree and a right tree.
You give your recursion two nodes or "two root nodes" and go down the tree at the same time.
Just remember the question is about being a mirror.

```
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        def is_symmetric_helper(root, other_node):
            if root is None and other_node is None:
                return True
            if root is None or other_node is None:
                return False
            return is_symmetric_helper(root.left, other_node.right) \
                and is_symmetric_helper(root.right, other_node.left) \
                and root.val == other_node.val
        return is_symmetric_helper(root, root)
```

## Iterative Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of nodes in tree

```
class Solution:
    def isSymmetric(self, root: TreeNode) -> bool:
        stack = list([(root, root)])
        while len(stack) != 0:
            n1, n2 = stack.pop()
            if n1 is None and n2 is None:
                continue
            if n1 is None or n2 is None:
                return False
            if n1.val != n2.val:
                return False
            stack.append((n1.left, n2.right))
            stack.append((n1.right, n2.left))
        return True
```
