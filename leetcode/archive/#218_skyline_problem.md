https://briangordon.github.io/2014/08/the-skyline-problem.html

# SOLUTION
Simple solution but out of memory problem
```
class Solution(object):
    def getSkyline(self, buildings):
        """
        :type buildings: List[List[int]]
        :rtype: List[List[int]]
        """
        if len(buildings) == 0:
            return []
        heights = self.get_height_skyline_of(buildings) 
        min_left = buildings[0][0]
        return self.get_key_points_from(heights, min_left)
        
    def get_key_points_from(self, heights, min_left):
        result = list([min_left, heights[0]])
        last_height = heights[0]
        for index, height in enumerate(heights[1:], min_left+1):
            if last_height != height:
                prev_height = heights[index-1]
                if last_height > prev_height:
                    result.append([index-1, prev_height])
                    last_height = prev_height
                else:
                    result.append([index, height])
                    last_height = height
        if last_height == heights[-1]:
            result.append([len(heights)+min_left-1, 0])
        return result
                
    def get_height_skyline_of(self, buildings):
        min_left = min(buildings, key=lambda b: b[0])[0]
        max_right = max(buildings, key=lambda b: b[1])[1]
        n_x_coords = max_right - min_left + 1
        n_shifts = max_right - n_x_coords
        heights = [0] * n_x_coords
        
        for building in buildings:
            left, right, height = building
            start_index = left - n_shifts - 1
            end_index = right - n_shifts - 1
            for index in range(start_index, end_index+1):
                heights[index] = max(heights[index], height)
        return heights
```
