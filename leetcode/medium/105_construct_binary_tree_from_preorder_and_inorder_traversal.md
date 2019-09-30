# 105. Construct Binary Tree from Preorder and Inorder Traversal

## Recursive Solution
- Runtime: O(N)
- Space: O(N) (Due to hash table)
- N = Number of elements in list

The preorder traversal is the pinnacle element.
The property to be able to find the root node of the preorder traversal is key.

Given preorder of [A,B,D,E,C,F,G] and inorder of [D,B,E,A,F,C,G].
Using the preorder, we can see that A is the root of the entire tree.
Using A, looking at the inorder we know that the left-subtree is [D,B,E] and the right-subtree is [F,C,G].
Going back to the preorder with this information, we can see that [B,D,E] is our left-subtree, and [C,F,G] is our right.

Now we can then use recursion to further build the nodes, we can take preorder [B,D,E] and inorder [D,B,E] to build the left-subtree, to similar effect with the right as well.
Using the same rules applied above, we know B is the root node and [D] is on the left and [E] is on the right.

To find the associated index of the inorder value using the preorder value would take O(N), however, using an enumerated hash table can make this O(1).

```
class Solution:
    def buildTree(self, preorder: List[int], inorder: List[int]) -> TreeNode:
        
        def build_helper(preorder_start, preorder_end, inorder_start, inorder_end):
            if preorder_start > preorder_end or inorder_start > inorder_end:
                return None
            inorder_root_idx = val_to_inorder_idx[preorder[preorder_start]]
            left_size = inorder_root_idx - inorder_start
            node = TreeNode(preorder[preorder_start])
            node.left = build_helper(preorder_start+1, preorder_start+1+left_size, inorder_start, inorder_root_idx-1)
            node.right = build_helper(preorder_start+1+left_size, preorder_end, inorder_root_idx+1, inorder_end)
            return node
        
        val_to_inorder_idx = {val: i for i, val in enumerate(inorder)}
        return build_helper(0, len(preorder)-1, 0, len(inorder)-1)
```
