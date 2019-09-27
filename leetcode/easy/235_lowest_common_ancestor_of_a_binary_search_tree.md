# 235. Lowest Common Ancestor of a Binary Search Tree

## Iterative Solution

- Runtime: O(H)
- Space: O(H)
- H = Height of the tree

I recommend doing question 236 on a binary tree first before doing this question.
Compared to a binary tree, there are some optimizations we can do to improve the solution.

Since this is a BST, we can easily figure out where p and q are in relation to the root node.
Using this, we can build the following intuition.
- If p and q exist on separate sub-trees, left and right, then the root must be the LCA.
- If p and q both exist on the right, we should traverse right.
- If p and q both exist on the left, we should traverse left.
- If the root is p or q, for example root is p, since we are traversing towards p and q, q is also in one of our sub-trees, this means root must be LCA.

```
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        curr = root
        if p.val > q.val: # p always smaller than q
            p, q = q, p
        while curr:
            if p.val < curr.val and q.val > curr.val: # p and q exists on both sub-trees
                return curr
            elif curr is p or curr is q: # root is p or q
                return curr
            elif p.val < curr.val and q.val < curr.val: # p and q both exists on the left sub-tree
                curr = curr.left
            else: # p and q both exists on the right sub-tree
                curr = curr.right
        return root
```
