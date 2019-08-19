# 437. Path Sum III

## Top Down Recursive Solution
- Runtime: O(N*H)
- Space: O(H*H)
- H = Height of tree

You can do a brute force solution with a dfs down the tree for each node, but at run-time O(N\*N).
If you get a linked list as a tree, its essentially for each node, visit all the other nodes below.
You can get a better run-time by using a dictionary, you can do this either top down or bottom up approach, this example I did top-down.
Both approaches will have the same run-time and space complexities.

The dictionary will act like a memoization, what paths do we have so far?
For each node, we will add our node's value to the exisiting paths then check if the target exists in there.
The reason why we need a dictionary is that we are storing the path's sum as the key and the number of times this sum can occur.
As you are traversing down the tree, you need a way to check if you've seen the target before.
When you get to the leaf nodes, there can be multiple duplicate sums, that is why a dictionary is used over a set.

The run-time can be a bit tricky to calculate, since we need to add the current node's value to the dictionary, we have to iterate the dictionary.
The largest size the dictionary can be is of height of the tree, however, we have to visit every node.
So at most, we need O(Height of tree) space and O(Number of nodes * height of tree) run-time.

```
from collections import defaultdict

class Solution:
    def pathSum(self, root: TreeNode, target: int) -> int:
        def path_sum_helper(root, target, path_to_count):
            if root is None:
                return 0
            new_paths = defaultdict(int)
            for key, val in path_to_count.items():
                new_paths[root.val+key] += val
            new_paths[root.val] += 1
            left_n_paths = path_sum_helper(root.left, target, new_paths)
            right_n_paths = path_sum_helper(root.right, target, new_paths)
            return left_n_paths + right_n_paths + new_paths[target]
        
        return path_sum_helper(root, target, defaultdict(int))
```
