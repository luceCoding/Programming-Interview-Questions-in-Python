# 112. Path Sum

## Brute Force Recursive Solution
- Runtime: O(3^N)
- Space: O(3^N)
- N = Number of nodes in tree

Thinking about the brute force method, for every node in the tree, traverse their left child and right child to find the target sum.
Due to the natural of the problem, we have to traverse all the nodes down the path until we reach a leaf node.
After we know the number of paths for the left child and right child, we have to repeat what we did but now the left child is the parent and again for the right child as parent.
Because of this, we need to keep a visited set to make sure we don't add duplicate paths.
This is similar to a DFS implementation.

The runtime can be calculated in this way. 
For each recursive call, we want to traverse the left and right children down to the leaf nodes, this is O(N) runtime. 
Since we would have to do this again but for the left and right children, it about O(N-1) for each.
So we basically have O(N) three times which means O(3^N).

Remember the formula for recursion, (Number of calls ^ (Big O per call))

```
class Solution:
    def pathSum(self, root: TreeNode, target: int) -> int:
        def path_sum_helper(root, curr_sum, target, parent, visited):
            if root is None:
                return 0
            curr_sum += root.val
            n_paths = 0
            if curr_sum == target and (parent, root) not in visited:
                visited.add((parent, root))
                n_paths += 1
            n_paths += path_sum_helper(root.left, curr_sum, target, parent, visited)
            n_paths += path_sum_helper(root.right, curr_sum, target, parent, visited)
            n_paths += path_sum_helper(root.left, 0, target, root.left, visited)
            n_paths += path_sum_helper(root.right, 0, target, root.right, visited)
            return n_paths
        
        return path_sum_helper(root, 0, target, parent=root, visited=set())
```
