# 329. Longest Increasing Path in a Matrix

## Memo and DFS Solution
- Run-time: O(V + E)
- Space: O(V)
- V = Vertices
- E = Edges

Simple solution is to perform a DFS for each element in the matrix.
That would be a O(V + E)^2 run-time.
We can further improve the run-time by using memoization to reduce the need to recalculate the same element over again.
Therefore, each element in the matrix will only have one DFS performed on it.
The memoization will keep the longest increasing path for each (i, j).

Unlike a traditional DFS that uses a visited set, we can save more space by not using one here.
As the question is worded, we only care about visiting elements that are increasing.
Therefore, a cycle is not possible in this case, no need for a visited set.

```
class Solution:
    def longestIncreasingPath(self, matrix: List[List[int]]) -> int:

        def get_neighbors(r, c):
            dirs = [(0, 1), (1, 0), (0, -1), (-1, 0)]
            for _r, _c in dirs:
                _r += r
                _c += c
                if 0 <= _r < len(matrix) and 0 <= _c < len(matrix[0]):
                    yield (_r, _c)

        def dfs(i, j):
            if (i, j) in memo:
                return memo[(i, j)]
            longest = 1
            for _i, _j in get_neighbors(i, j):
                if matrix[_i][_j] > matrix[i][j]:
                    longest = max(longest, dfs(_i, _j) + 1)
            memo[(i , j)] = longest
            return longest

        longest = 0
        memo = dict()
        for i in range(len(matrix)):
            for j in range(len(matrix[0])):
                longest = max(longest, dfs(i, j))
        return longest
```
