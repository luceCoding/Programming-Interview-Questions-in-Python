# SOLUTION (Time Limit Exceeded)
```
class Solution(object):
    def shortestDistance(self, grid):
        """
        :type grid: List[List[int]]
        :rtype: int
        """
        n_buildings = sum(x.count(1) for x in grid)
        bfs_queue = collections.deque()
        result = -1
        
        for row_index in range(0, len(grid)):
            for col_index in range(0, len(grid[0])):
                element = grid[row_index][col_index]
                if element == 0: # empty land
                    start_indexes = (row_index, col_index)
                    local_result = self.bfs_all_buildings(start_indexes, n_buildings, grid)
                    if local_result != -1:
                        if result == -1:
                            result = local_result
                        result = min(result, local_result)
        return result
    
    def bfs_all_buildings(self, start_indexes, n_buildings, grid):
        bfs_queue = collections.deque()
        bfs_queue.appendleft(start_indexes)
        visited_set = set()
        visited_set.add(start_indexes)
        distance = result = 0
        total_n_buildings_found = 0
        
        while len(bfs_queue) != 0 and total_n_buildings_found < n_buildings:
            n_pops = len(bfs_queue)
            distance += 1
            n_buildings_found = 0
            while n_pops > 0:
                curr_indexes = bfs_queue.pop()
                n_pops -= 1
                neighbors = self.get_neighbors(curr_indexes, grid)
                for neighbor in neighbors:
                    if neighbor not in visited_set:
                        element = grid[neighbor[0]][neighbor[1]]
                        visited_set.add(neighbor)
                        if element == 0:
                            bfs_queue.appendleft(neighbor)
                        elif element == 1:
                            n_buildings_found += 1
                            total_n_buildings_found += 1
                if n_buildings_found != 0:
                    result += (distance * n_buildings_found)
                    n_buildings_found = 0
        if total_n_buildings_found != n_buildings:
            return -1
        return result
    
    def get_neighbors(self, indexes, grid):
        directions = [(0,-1),(1,0),(0,1),(-1,0)]
        results = list()
        for direction in directions:
            next_indexes = (indexes[0]+direction[0], indexes[1]+direction[1])
            if self.within_bounds(next_indexes, grid):
                results.append(next_indexes)
        return results

    def within_bounds(self, indexes, grid):
        row_i = indexes[0]
        col_i = indexes[1]
        if row_i >= 0 and col_i >= 0 and row_i < len(grid) and col_i < len(grid[0]):
            return True
        return False
```
