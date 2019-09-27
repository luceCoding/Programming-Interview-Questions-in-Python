# 236. Lowest Common Ancestor of a Binary Tree

## Recursive Solution

- Runtime: O(N)
- Space: O(H)
- N = Number of nodes in tree
- H = Height of tree

From the perspective of a root node, there are only three options to consider.
- The LCA exists on the left sub-tree.
- The LCA exists on the right sub-tree.
- The LCA is the root node.

To figure out if an LCA exists would mean we need to find p and q in either sub-tree.
If either are found, we have to let the parent know of their existence.

The other question is when to evaluate these conditions.
We generally don't want to traverse a sub-tree if the LCA is already found, so if the recursion function returns the LCA, we should instead return it back up the tree.
Secondly, if LCA has not been found from either side, we need to know if either p or q were found.
So a number would be returned to the parent node.
With this number, we can check if our root is p or q and add it with the returned number found.
If the number happens to be 2, we found the LCA.
In summary, we need a post-order traversal recursion call.

The worst case is that we have to traverse the entire tree to find p and q. However, we will never need more than height of the tree O(H) space to find p or q.

```
from collections import namedtuple

LCA = namedtuple('LCA', ['n_found', 'lca'])

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':

        def LCA_helper(root):
            if root is None:
                return LCA(n_found=0, lca=None)
            left = LCA_helper(root.left)
            if left.n_found == 2:
                return left
            right = LCA_helper(root.right)
            if right.n_found == 2:
                return right
            n_found = left.n_found + right.n_found + (1 if root is p or root is q else 0)
            return LCA(n_found=n_found, lca=root if n_found == 2 else None)

        return LCA_helper(root).lca
```
