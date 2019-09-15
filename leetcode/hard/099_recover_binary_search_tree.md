# 99. Recover Binary Search Tree

## In-order Solution
- Runtime: O(N)
- Space: O(H)
- N = Number of nodes in BST
- H = Height of BST

This was asked on an amazon phone screen interview question.

This can be difficult to get, you would have to know that out of the three traversals, in-order traversal actually traverses the BST in sorted order.
Using this, we can traverse the BST in in-order and compare the current node to the previous node.
If either are out of sort, we can determine which are the swapped nodes.

```
class Solution:
    prev = TreeNode(-sys.maxsize-1)
    node1 = None
    node2 = None
    
    def recoverTree(self, root: TreeNode) -> None:
        
        def recover_helper(root):
            if root is None:
                return
            recover_helper(root.left)
            if self.node1 is None and self.prev.val >= root.val:
                self.node1 = self.prev
            if self.node1 is not None and self.prev.val >= root.val:
                self.node2 = root
            self.prev = root
            recover_helper(root.right)
        
        recover_helper(root)
        self.node1.val, self.node2.val = self.node2.val, self.node1.val
        return root
```
