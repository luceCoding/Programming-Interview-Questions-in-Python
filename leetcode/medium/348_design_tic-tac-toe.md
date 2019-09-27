# 348. Design Tic-Tac-Toe

## Solution

- Runtime: O(N)
- Space: O(N)
- N = Given N

This is a fair production code quality example.
Make sure when you code, you break down your methods else you may fail this question.

To achieve O(N) run-time, we can use a summation system for each row, column and the two diagonals.
Player 1 will increment by 1 and player 2 will decrement by 1.
This means that for any given row, column or diagonal, if either equal N or -N, there is a straight line of the same player.

```
# (0,0) (0,1) (0,2) (0,3)
# (1,0) (1,1) (1,2) (1,3)
# (2,0) (2,1) (2,2) (2,3)
# (3,0) (3,1) (3,2) (3,3)

class TicTacToe:

    def __init__(self, n: int):
        """
        Initialize your data structure here.
        """
        self.row_sum = [0] * n
        self.col_sum = [0] * n
        self.diagonal1 = 0
        self.diagonal2 = 0
        self.n = n

    def move(self, row: int, col: int, player: int) -> int:
        """
        Player {player} makes a move at ({row}, {col}).
        @param row The row of the board.
        @param col The column of the board.
        @param player The player, can be either 1 or 2.
        @return The current winning condition, can be either:
                0: No one wins.
                1: Player 1 wins.
                2: Player 2 wins.
        """
        increment = (1 if player == 1 else -1)
        self.row_sum[row] += increment
        self.col_sum[col] += increment
        if row == col:
            self.diagonal1 += increment
        if row + col == self.n-1:
            self.diagonal2 += increment
        if any([self.check_horizontal(player, row), self.check_vertical(player, col), self.check_diagonals(player, row, col)]):
            return player
        return 0

    def check_horizontal(self, player, row):
        return abs(self.row_sum[row]) == self.n

    def check_vertical(self, player, col):
        return abs(self.col_sum[col]) == self.n

    def check_diagonals(self, player, row, col):
        return abs(self.diagonal1) == self.n or abs(self.diagonal2) == self.n

# Your TicTacToe object will be instantiated and called as such:
# obj = TicTacToe(n)
# param_1 = obj.move(row,col,player)
```
