# 778. Swim in Rising Water

## Heap Solution
- Runtime: O(T * Nlog(N))
- Space: O(N)
- N = Number of elements in grid
- T = Time

You may have thought to use BFS for this question, but the problem is the fact that we cannot swim to an element in the grid until the water is at its level.
So we have a second issue to figure out which elements are up to the water's level.
Since we are not finding the shortest path exactly, just the shortest time needed to reach grid[N-1][N-1].

To find the elements within the water's level, a min heap comes as the obvious choice.
We also need to avoid adding the same element in the heap, so a visited set will be required.

The logic goes as follows:
1. As long as there are elements in the heap.
2. Increment time + 1.
2. Continue popping off the min heap until the top of the heap is > time.
2. For each popped off element, if the element popped off is the last element, return current time.
2. Add all neighbors of the popped elements into the min heap. Keep a visited set to not add duplicate elements into the heap.
5. Repeat.

```
class Square(object):
    
    def __init__(self, elevation, x, y):
        self.elevation = elevation
        self.x = x
        self.y = y
        
    def __lt__(self, other):
        return self.elevation < other.elevation

class Solution(object):
    def swimInWater(self, grid):
        
        def get_neighbors(x, y):
            directions = [(0, 1), (1, 0), (-1, 0), (0, -1)]
            for _x, _y in directions:
                _x += x
                _y += y
                if _x >= 0 and _x < len(grid) and _y >= 0 and _y < len(grid):
                    yield (_x, _y)
        
        if len(grid) == 0 or len(grid[0]) == 0:a
            return 0
        min_heap = list([Square(grid[0][0], 0, 0)])
        visited = set([(0, 0)])
        time = 0
        while len(min_heap) != 0:
            time += 1
            while len(min_heap) != 0 and min_heap[0].elevation <= time:
                popped_square = heapq.heappop(min_heap)
                if popped_square.x == len(grid)-1 and popped_square.y == len(grid)-1:
                    return time
                for _x, _y in get_neighbors(popped_square.x, popped_square.y):
                    if (_x, _y) not in visited:
                        visited.add((_x, _y))
                        heapq.heappush(min_heap, Square(grid[_x][_y], _x, _y))
        return 0
```
