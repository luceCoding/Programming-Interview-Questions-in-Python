# QUESTION
Given a robot cleaner in a room modeled as a grid.

Each cell in the grid can be empty or blocked.

The robot cleaner with 4 given APIs can move forward, turn left or turn right. Each turn it made is 90 degrees.

When it tries to move into a blocked cell, its bumper sensor detects the obstacle and it stays on the current cell.

Design an algorithm to clean the entire room using only the 4 given APIs shown below.
```
interface Robot {
  // returns true if next cell is open and robot moves into the cell.
  // returns false if next cell is obstacle and robot stays on the current cell.
  boolean move();

  // Robot will stay on the same cell after calling turnLeft/turnRight.
  // Each turn will be 90 degrees.
  void turnLeft();
  void turnRight();

  // Clean the current cell.
  void clean();
}
```
Example:
```
Input:
room = [
  [1,1,1,1,1,0,1,1],
  [1,1,1,1,1,0,1,1],
  [1,0,1,1,1,1,1,1],
  [0,0,0,1,0,0,0,0],
  [1,1,1,1,1,1,1,1]
],
row = 1,
col = 3

Explanation:
All grids in the room are marked by either 0 or 1.
0 means the cell is blocked, while 1 means the cell is accessible.
The robot initially starts at the position of row=1, col=3.
From the top left corner, its position is one row below and three columns right.
```
Notes:
- The input is only given to initialize the room and the robot's position internally. You must solve this problem "blindfolded". In other words, you must control the robot using only the mentioned 4 APIs, without knowing the room layout and the initial robot's position.
- The robot's initial position will always be in an accessible cell.
- The initial direction of the robot will be facing up.
- All accessible cells are connected, which means the all cells marked as 1 will be accessible by the robot.
- Assume all four edges of the grid are all surrounded by wall.

# EXPLANATION
This question is testing your knowledge in writing clean code while using someone else's API and your understanding of DFS/stack.
This question can get very messy fast, so its best to keep it simple.

First, noticed that your robot will be placed in a room/grid at random. Instead, think about how you would clean/visit all the cells in the grid if you did know your coordinates?
You should think about DFS, BFS however would be more difficult to implement. As with any DFS, you would need a visited set to avoid cycles. 

Now lets consider the fact that your robot doesn't know where its coordinates are at. Can we instead use arbitrary coordinates?
Say we always start at (0,0). Then base on our direction, say we pick an arbitrary direction, up. We attempt to move up to (0,1) based on our current direction.
If that doesn't work, we rotate to the right and try (1,0). For each cell we will attempt to try the next cell in a clockwise manner, counter-clockwise can also work.

Now the next question is, what happens after we have tried all four cells. Our robot should then return to the original cell it came from. 

It is also important to note that when your robot leaves a cell, its direction is different from when it leaves the cell then when it comes back. So you need to reset its direction.

# SOLUTION
```
# """
# This is the robot's control interface.
# You should not implement it, or speculate about its implementation
# """
#class Robot(object):
#    def move(self):
#        """
#        Returns true if the cell in front is open and robot moves into the cell.
#        Returns false if the cell in front is blocked and robot stays in the current cell.
#        :rtype bool
#        """
#
#    def turnLeft(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def turnRight(self):
#        """
#        Robot will stay in the same cell after calling turnLeft/turnRight.
#        Each turn will be 90 degrees.
#        :rtype void
#        """
#
#    def clean(self):
#        """
#        Clean the current cell.
#        :rtype void
#        """

"""
       (0,1)    
(-1,0) (0,0) (1,0)    
       (0,-1) 
"""

class Solution(object):
    def __init__(self):
        # represents up, right, down, left directions
        self.dirs = [(0,1), (1,0), (0,-1), (-1,0)]

        # current direction of the roomba based on an arbitrary direction 'up'
        self.curr_dir = self.dirs[0]
        
        # current arbitrary cell
        self.curr_pos = (0,0)
        
        # cleaned cells
        self.visited_set = set()
    
    def cleanRoom(self, robot):
        """
        :type robot: Robot
        :rtype: None
        """
        self.dfs_clean(robot)

    def dfs_clean(self, robot):
        robot.clean()
        curr_pos = self.curr_pos
        self.visited_set.add(self.curr_pos)
        
        # continue moving forward
        self.move_forward(robot)
        
        # right
        self.rotate_right(robot)
        self.move_forward(robot)
        
        # bottom
        self.rotate_right(robot)
        self.move_forward(robot)
        
        # left
        self.rotate_right(robot)
        self.move_forward(robot)
        
        # back track and reset direction
        self.rotate_right(robot)
        self.rotate_right(robot)
        self.rotate_right(robot)
        self.move_back(robot)
        self.rotate_right(robot)
        self.rotate_right(robot)
        
    def move_back(self, robot):
        next_pos = self.get_next_pos()
        if robot.move():
            self.curr_pos = next_pos
        
    def move_forward(self, robot):
        next_pos = self.get_next_pos()
        if next_pos not in self.visited_set:
            if robot.move():
                self.curr_pos = next_pos
                self.dfs_clean(robot)
            else: # hit a wall
                self.visited_set.add(next_pos)
        
    def get_next_pos(self):
        next_pos = (self.curr_pos[0]+self.curr_dir[0], self.curr_pos[1]+self.curr_dir[1])
        return next_pos
    
    def rotate_right(self, robot):
        for index, direction in enumerate(self.dirs):
            if direction == self.curr_dir:
                if index >= len(self.dirs)-1:
                    self.curr_dir = self.dirs[0]
                else:
                    self.curr_dir = self.dirs[index+1]
                break
        robot.turnRight()
```
