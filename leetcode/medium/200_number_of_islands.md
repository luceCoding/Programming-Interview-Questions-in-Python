# 200. Number of Islands

## DFS Recursive Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in grid

By going around the grid, we can DFS on the first '1' encountered per island.
The DFS will only call the recursion on neighboring elements if the element is a '1', else skip it.

We can save some space by reusing the given the grid.
You should ask the interviewer if you are allowed to modify the original grid.
We can then use another number such as "2" to represented an already visited island, therefore, no longer needing a visited set during our BFS or DFS.

```
class Solution(object):
    def numIslands(self, grid):

        def dfs(r, c):
            grid[r][c] = '2'
            for x, y in get_neighbors(r, c):
                if grid[x][y] == '1':
                    dfs(x, y)

        def get_neighbors(x, y):
            dirs = [(1,0),(0,1),(-1,0),(0,-1)]
            for _x, _y in dirs:
                _x += x
                _y += y
                if 0 <= _x < len(grid) and 0 <= _y < len(grid[0]):
                    yield (_x, _y)

        n_islands = 0
        for r, row in enumerate(grid):
            for c, col in enumerate(row):
                if col == '1':
                    n_islands += 1
                    dfs(r, c)
        return n_islands
```

## BFS Iterative Solution
- Runtime: O(N * M)
- Space: O(N * M)
- N = Number of Rows
- M = Number of Columns

Remember BFS uses a queue, gathering all the neighboring elements and adding them into the queue.

```
from collections import deque

class Solution(object):
    def numIslands(self, grid):

        def get_neighbors(x, y):
            dirs = [(1,0),(0,1),(-1,0),(0,-1)]
            for _x, _y in dirs:
                _x += x
                _y += y
                if 0 <= _x < len(grid) and 0 <= _y < len(grid[0]):
                    yield (_x, _y)

        n_islands = 0
        for r, row in enumerate(grid):
            for c, col in enumerate(row):
                if col == '1':
                    n_islands += 1
                    grid[r][c] == '2'
                    queue = deque([(r, c)])
                    while queue:
                        x, y = queue.pop()
                        for _x, _y in get_neighbors(x, y):
                            if grid[_x][_y] == '1':
                                grid[_x][_y] = '2'
                                queue.appendleft((_x, _y))
        return n_islands
```
