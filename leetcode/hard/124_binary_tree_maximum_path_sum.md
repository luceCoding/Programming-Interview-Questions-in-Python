# 124. Binary Tree Maximum Path Sum

## Recursive Solution
- Runtime: O(N)
- Space: O(H)
- N = Nodes in tree
- H = Height of tree

Consider the solution in the perspective of a single node.
If you wanted to figure out which path was the largest there are 4 cases.
1. Only my node.
2. The left path and my node.
3. The right path and my node.
4. The left and right paths and my node.

After that, you should always return the largest left or right path to your parent node.

```
class Solution:
    def __init__(self):
        self.max_path = float('-inf')
    
    def maxPathSum(self, root: TreeNode) -> int:
        def path_sum_helper(root):
            if root is None:
                return 0
            left_path = path_sum_helper(root.left)
            right_path = path_sum_helper(root.right)
            self.max_path = max(self.max_path,
                                root.val,
                                left_path+root.val,
                                right_path+root.val,
                                left_path+root.val+right_path)
            return max(left_path+root.val, right_path+root.val, root.val)
        
        path_sum_helper(root)
        return self.max_path
```
