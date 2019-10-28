# 307. Range Sum Query - Mutable

## Segment Tree Solution
- Run-time: create_tree() is O(N), sumRange() is O(logN), update() is O(logN)
- Space: O(N)
- N = Number of given nums

A segment tree is similar to a binary tree, in fact, the only difference is that segment trees traversal via. the range of indexes.
Each node of the tree, instead of storing a value, stores the values between the range of indexes.
Segment trees are good if you have a lot of numbers and need to either find the max, min, sum, etc... of a given range.

Naive methods would require O(N) time traversal to find the result of a given range.
Other methods use a 2d array of O(N^2) space and O(N^2) run-time to pre-process every range but O(1) look up after.
These methods don't work well when there are billions of numbers, therefore, segment trees are a great alternative.

```
class NumArray:

    def __init__(self, nums: List[int]):
        self.tree = SegmentTree(nums)

    def update(self, i: int, val: int) -> None:
        self.tree.update(i, val)

    def sumRange(self, i: int, j: int) -> int:
        return self.tree.get_range(i, j)

class SegmentTree(object):

    def __init__(self, nums):
        self.root = self._create_tree(nums)

    def _create_tree(self, nums):

        def tree_builder(left, right):
            if left > right:
                return None
            if left == right:
                return Node(nums[left], left, right)
            mid_idx = (left + right) // 2
            left_node = tree_builder(left, mid_idx)
            right_node = tree_builder(mid_idx + 1, right)
            total_sum = left_node.sum if left_node is not None else 0
            total_sum += right_node.sum if right_node is not None else 0
            new_node = Node(total_sum, start=left, end=right, left=left_node, right=right_node)
            return new_node

        return tree_builder(0, len(nums) - 1)

    def update(self, idx, val):

        def update_helper(curr):
            if curr.start_idx == curr.end_idx == idx: # leaf
                curr.sum = val
                return
            mid_idx = (curr.start_idx + curr.end_idx) // 2
            if idx <= mid_idx: # go left
                update_helper(curr.left)
            else: # go right
                update_helper(curr.right)
            curr.sum = curr.left.sum + curr.right.sum

        update_helper(self.root)

    def get_range(self, l, r):

        def sum_helper(curr, left, right):
            if left == curr.start_idx and right == curr.end_idx: # total overlap
                return curr.sum
            mid_idx = (curr.start_idx + curr.end_idx) // 2
            if right <= mid_idx: # range is only on the left subtree?
                return sum_helper(curr.left, left, right)
            elif left >= mid_idx + 1:  # range is only on the right subtree?
                return sum_helper(curr.right, left, right)
            # ranges are in both left and right subtrees
            return sum_helper(curr.left, left, mid_idx) + sum_helper(curr.right, mid_idx + 1, right)

        return sum_helper(self.root, l, r)

class Node(object):

    def __init__(self, _sum, start, end, left=None, right=None):
        self.start_idx = start
        self.end_idx = end
        self.sum = _sum
        self.left = left
        self.right = right
```
