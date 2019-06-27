# 200. Number of Islands

## Traditional DFS Recursive
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
            yield x, y-1
            yield x, y+1
            yield x-1, y
            yield x+1, y
                
        def within_bounds(x, y, grid):
            if y >= 0 and y < len(grid) and x >= 0 and x < len(grid[0]):
                return True
            return False
        
        return traverse_helper(grid)
```
