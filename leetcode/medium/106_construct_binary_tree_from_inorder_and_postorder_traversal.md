# 106. Construct Binary Tree from Inorder and Postorder Traversal

## Recursive Solution
- Runtime: O(N)
- Space: O(N) (Due to hash table)
- N = Number of elements in list

Similar to question 105.
Postorder allows us to know which is the root node by using the last element of the array.
With this, we can figure out the left and right sub-trees in the inorder traversal.
Using recursion, we can continue to break up the sub-trees once we know which is the root of the sub-tree using this method.

To allow for quicker look ups for roots, we can build an enmuerated hash table to find the indexes for each value of the inorder traversal.

```
class Solution:
    def buildTree(self, inorder: List[int], postorder: List[int]) -> TreeNode:
        
        def build_helper(inorder_start, inorder_end, postorder_start, postorder_end):
            if inorder_start > inorder_end or postorder_start > postorder_end:
                return None
            inorder_root_idx = val_to_inorder_idx[postorder[postorder_end]]
            right_size = inorder_end - inorder_root_idx
            node = TreeNode(postorder[postorder_end])
            node.left = build_helper(inorder_start, inorder_root_idx-1, postorder_start, postorder_end - right_size - 1)
            node.right = build_helper(inorder_root_idx + 1, inorder_end, postorder_end - right_size, postorder_end - 1)
            return node
        
        val_to_inorder_idx = {val: i for i, val in enumerate(inorder)}
        return build_helper(0, len(inorder)-1, 0, len(postorder)-1)
```
