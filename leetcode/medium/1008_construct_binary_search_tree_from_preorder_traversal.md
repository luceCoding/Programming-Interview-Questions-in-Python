## Recursive Solution with Ranges

- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

I've selected this question because it would be good to know at least one reconstruction method of a BST.
Preorder traversal was selected because it is one of the easier ones to understand.
Unlike the other traversals, preorder has a property of knowing what the root node is by looking at the first element of the list.

We can use the fact that we know what the root node is and set a upper and lower bound for the values in the left and right recursion calls. When we are out of bounds for either recursion, we now know we have reached the furthest possible value in the list and backtrack up to the parent node.

```
class Solution:
    curr_idx = 0
    def bstFromPreorder(self, preorder: List[int]) -> TreeNode:
        def bst_creator(lower, upper):
            if self.curr_idx >= len(preorder):
                return None
            root_val = preorder[self.curr_idx]
            if not lower <= root_val < upper:
                return None
            new_node = TreeNode(root_val)
            self.curr_idx += 1
            left_node = bst_creator(lower, root_val)
            right_node = bst_creator(root_val, upper)
            new_node.left = left_node
            new_node.right = right_node
            return new_node
        
        return bst_creator(float('-inf'), float('inf'))
```
