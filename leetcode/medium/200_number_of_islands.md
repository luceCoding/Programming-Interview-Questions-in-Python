# 200. Number of Islands

## Traditional DFS Recursive Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in grid

Using a visited set and recursion to achieve a solution.
```
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        def traverse_helper(grid):
            n_islands = 0
            visited = set()
            for y_index, y in enumerate(grid):
                for x_index, x in enumerate(y):
                    if (x_index, y_index) not in visited and x == '1':
                        n_islands += 1
                        traverse_islands_dfs_recursion(x_index, y_index, grid, visited)
            return n_islands
                    
        def traverse_islands_dfs_recursion(x, y, grid, visited):
            if not within_bounds(x, y, grid) or (x,y) in visited or grid[y][x] == '0':
                return
            visited.add((x,y))
            for neighbor_x, neighbor_y in get_neighbors_gen(x, y, grid):
                traverse_islands_dfs_recursion(neighbor_x, neighbor_y, grid, visited)
                
        def get_neighbors_gen(x, y, grid):
            yield x, y-1 # top
            yield x, y+1 # bottom
            yield x-1, y # left
            yield x+1, y # right
                
        def within_bounds(x, y, grid):
            if y >= 0 and y < len(grid) and x >= 0 and x < len(grid[0]):
                return True
            return False
        
        return traverse_helper(grid)
```

## Traditional BFS Iterative Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of elements in grid

Similar concept to the previous DFS recursion but now using BFS and a stack.
```
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        def traverse_helper(grid):
            n_islands = 0
            visited = set()
            for y_index, y in enumerate(grid):
                for x_index, x in enumerate(y):
                    if (x_index, y_index) not in visited and x == '1':
                        n_islands += 1
                        traverse_islands_bfs_iterative(x_index, y_index, grid, visited)
            return n_islands
                    
        def traverse_islands_bfs_iterative(x, y, grid, visited):
            stack = list()
            stack.append((x,y))
            while len(stack) > 0:
                x_index, y_index = stack.pop()
                visited.add((x_index, y_index))
                for x_neighbor, y_neighbor in get_neighbors_gen(x_index, y_index, grid):
                    if within_bounds(x_neighbor, y_neighbor, grid) \
                        and (x_neighbor, y_neighbor) not in visited \
                        and grid[y_neighbor][x_neighbor] == '1':
                            stack.append((x_neighbor, y_neighbor))
                
        def get_neighbors_gen(x, y, grid):
            yield x, y-1 # top
            yield x, y+1 # bottom
            yield x-1, y # left
            yield x+1, y # right
                
        def within_bounds(x, y, grid):
            if y >= 0 and y < len(grid) and x >= 0 and x < len(grid[0]):
                return True
            return False
        
        return traverse_helper(grid)
```

## O(1) Space BFS Iterative Solution
- Runtime: O(N)
- Space: O(1)
- N = Number of elements in grid

We can achieve O(1) space by reusing the given the grid. You should ask the interviewer if you are allowed to modify the original grid. We can then use another number such as "-1" to represented an already visited island, therefore, no longer needing a visited set during our BFS or DFS.
```
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        def traverse_helper(grid):
            n_islands = 0
            for y_index, y in enumerate(grid):
                for x_index, x in enumerate(y):
                    if x == '1':
                        n_islands += 1
                        traverse_islands_bfs_iterative(x_index, y_index, grid)
            return n_islands
                    
        def traverse_islands_bfs_iterative(x, y, grid):
            stack = list()
            stack.append((x,y))
            while len(stack) > 0:
                x_index, y_index = stack.pop()
                grid[y_index][x_index] = '-1'
                for x_neighbor, y_neighbor in get_neighbors_gen(x_index, y_index, grid):
                    if within_bounds(x_neighbor, y_neighbor, grid) \
                        and grid[y_neighbor][x_neighbor] == '1':
                            stack.append((x_neighbor, y_neighbor))
                
        def get_neighbors_gen(x, y, grid):
            yield x, y-1 # top
            yield x, y+1 # bottom
            yield x-1, y # left
            yield x+1, y # right
                
        def within_bounds(x, y, grid):
            if y >= 0 and y < len(grid) and x >= 0 and x < len(grid[0]):
                return True
            return False
        
        return traverse_helper(grid)
```
