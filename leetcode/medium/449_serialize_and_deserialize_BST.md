# 449. Serialize and Deserialize BST

## Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of nodes in tree

We can build and save the tree with either preorder or postorder traversals.
This solution reuses the solution from question 1008.

```
class Codec:

    curr_idx = 0

    def serialize(self, root):

        def get_preorder(root):
            if root is None:
                return ''
            return ','.join([str(root.val), get_preorder(root.left), get_preorder(root.right)])

        return get_preorder(root)


    def deserialize(self, data):

        def get_bst_from_preorder(preorder):

            def bst_builder(left_bound=float('-inf'), right_bound=float('inf')):
                if self.curr_idx >= len(preorder) or not left_bound < preorder[self.curr_idx] <= right_bound:
                    return None
                val = preorder[self.curr_idx]
                root = TreeNode(preorder[self.curr_idx])
                self.curr_idx += 1
                root.left = bst_builder(left_bound, val)
                root.right = bst_builder(val, right_bound)
                return root

            preorder = [int(x) for x in preorder.split(',') if x != '']
            return bst_builder()

        return get_bst_from_preorder(data)
```
