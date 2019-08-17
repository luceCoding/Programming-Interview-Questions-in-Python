# 1091. Shortest Path in Binary Matrix

## BFS Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in grid

Generally, a BFS solution is best for these type of shortest path questions.
DFS can be done, however, DFS must visit all possible paths in the grid.
While BFS will always reach the shortest path first before visiting all possible paths, hence, a better run-time.

```
from collections import deque

class Solution:
    def shortestPathBinaryMatrix(self, grid: List[List[int]]) -> int:
        if len(grid) == 0 \
          or len(grid[0]) == 0 \
          or grid[0][0] == 1 \
          or grid[-1][-1] == 1:
            return -1
        bfs_queue = deque([(0,0)])
        grid[0][0] = -1
        shortest_length = 0
        while len(bfs_queue) != 0:
            n_pops = len(bfs_queue)
            shortest_length += 1
            for _ in range(n_pops):
                x, y = bfs_queue.popleft()
                if x == len(grid)-1 and y == len(grid[0])-1:
                    return shortest_length
                for _x, _y in self.get_neighbors(grid, x, y):
                    bfs_queue.append((_x, _y))
                    grid[_x][_y] = -1
        return -1
            
    def get_neighbors(self, grid, x, y):
        axises = [(1,0),(0,1),(0,-1),(-1,0),(-1,-1),(1,-1),(-1,1),(1,1)]
        for _x, _y in axises:
            _x += x
            _y += y
            if self.within_bounds(_x, _y, grid) and grid[_x][_y] == 0:
                yield (_x, _y)
                
    def within_bounds(self, x, y, grid):
        return x >= 0 and x < len(grid) and y >= 0 and y < len(grid[0])
```

## Bi-directional BFS Best Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in grid

We can further improve the run-time of the BFS by using a bi-directional BFS.
If you imagine a circle, every time you expand the circle, the number of neighbors being visited almost doubles.
Now if you used two circles and expanded them both simultaneously, when they touch, it would have visited less neighbors compared to using one circle.
This increases the run-time even more for some inputs, worst case, is the same as the first BFS solution.

```

```
