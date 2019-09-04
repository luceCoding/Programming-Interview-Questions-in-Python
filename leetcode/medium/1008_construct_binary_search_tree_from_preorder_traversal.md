## Recursive Solution

- Runtime: O(N^2)
- Space: O(1)
- N = Number of elements in array

I've selected this question because it would be good to know at least one reconstruction method of a BST.
Preorder traversal was selected because it is one of the easier ones to understand.
Unlike the other traversals, preorder has a property of knowing what the root node is by looking at the first element of the list.

Since we are reconstructing a BST and not a binary tree, we can identify which sections are the left and right subtree by comparing their values to the first element.
The left subtree will have values less/equal than the root and right subtree will have values greater than the root.

```
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def bstFromPreorder(self, preorder: List[int]) -> TreeNode:
        
        def bst_helper(preorder):
            if len(preorder) == 0:
                return None
            transition_index = len(preorder)
            for index, node in enumerate(preorder):
                if node > preorder[0]:
                    transition_index = index
                    break
            root = TreeNode(preorder[0])
            root.left = bst_helper(preorder[1:transition_index])
            root.right = bst_helper(preorder[transition_index:])
            return root
            
        return bst_helper(preorder)
```
