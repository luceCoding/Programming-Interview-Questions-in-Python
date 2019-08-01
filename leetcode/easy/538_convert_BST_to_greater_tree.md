## Recursive Solution
- Runtime: O(N)
- Space: O(H)
- N = Number of elements in tree
- H = Height of Tree

Take for example this BST:
```
     5
   /  \
  3    13
 / \   / \
1   4 8  15
```
This can also be represented by a sorted array:
```
[1,3,4,5,8,13,15]
```
If you think about this problem with the sorted array instead of a BST, how would you have solved this problem?
I would start from the right to left and add the number from the immediate right to my current number recursively, I would have this array for example.
```
[49,48,45,41,36,28,15]
```
To translate this into a BST solution, you would need to traverse to the right node first, then the left node last.
You would need to keep a 'global' integer value as a representation of the sum of great values. 
Since you are always traversing to the right of the BST, you will also be adding the greatest value first before any lower values are added to this integer.
You now know what are the greatest values at all times as you traverse.

```
class Solution:
    def __init__(self):
        self.greater_val_sum = 0

    def _convert_bst_helper(self, root):
        if root is None:
            return
        self._convert_bst_helper(root.right)
        self.greater_val_sum += root.val
        root.val = self.greater_val_sum
        self._convert_bst_helper(root.left)
            
    def convertBST(self, root: TreeNode) -> TreeNode:
        self._convert_bst_helper(root)
        return root
```

## Iterative Solution
- Runtime: O(N)
- Space: O(H)
- N = Number of elements in tree
- H = Height of Tree

```
class Solution:

    def convertBST(self, root: TreeNode) -> TreeNode:
        stack = list()
        curr_node = root
        greater_vals = 0
        while curr_node is not None or len(stack) > 0:
            if curr_node is not None:
                stack.append(curr_node)
                curr_node = curr_node.right
            elif len(stack) > 0: # At a leaf node
                curr_node = stack.pop()
                greater_vals += curr_node.val
                curr_node.val = greater_vals
                curr_node = curr_node.left
        return root
```
