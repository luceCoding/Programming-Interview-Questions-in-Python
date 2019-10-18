# 1091. Shortest Path in Binary Matrix

## BFS Solution
- Run-time: O(N)
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
- Run-time: O(N)
- Space: O(N)
- N = Number of elements in grid

We can further improve the run-time of the BFS by using a bi-directional BFS.
If you imagine a circle, every time you expand the circle, the number of neighbors being visited almost doubles.
Now if you used two circles and expanded them both simultaneously, when they touch, it would have visited less neighbors compared to using one circle.
This decreases the run-time for some inputs, worst case, is the same as the first BFS solution.

```
from collections import deque

class Solution(object):
    def shortestPathBinaryMatrix(self, grid):

        def get_neighbors(row_idx, col_idx):
            dirs = [(0,1),(1,0),(0,-1),(-1,0),(1,1),(-1,-1),(-1,1),(1,-1)]
            for r, c in dirs:
                r += row_idx
                c += col_idx
                if 0 <= r < len(grid) and 0 <= c < len(grid[0]):
                    yield (r, c)

        def bfs_once(queue, our_num, other_num):
            for _ in range(len(queue)):
                node = queue.pop()
                for x, y in get_neighbors(node[0], node[1]):
                    if grid[x][y] == other_num: # intersection
                        return True
                    elif grid[x][y] == 0:
                        grid[x][y] = our_num
                        queue.appendleft((x, y))
            return False

        if not grid or len(grid) == 0 or len(grid[0]) == 0: # is grid empty?
            return -1
        elif len(grid) == 1 and len(grid[0]) == 1: # does grid only contain one element?
            return 1
        elif grid[0][0] == 1 or grid[-1][-1] == 1: # are start or end blocked?
            return -1
        queue1 = deque([(0, 0)])
        queue2 = deque([(len(grid)-1, len(grid[0])-1)])
        grid[0][0], grid[-1][-1] = 2, 3 # numbers represent two visited sets
        n_moves = 2
        while queue1 and queue2:
            if bfs_once(queue1, 2, 3):
                return n_moves
            n_moves += 1
            if bfs_once(queue2, 3, 2):
                return n_moves
            n_moves += 1
        return -1
```
