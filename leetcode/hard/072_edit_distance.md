# 72. Edit Distance

## Levenshtein Distance
- Run-time: O(M*N)
- Space: O(M*N
- M = Length of word1
- N = Length of word2

The solution is called the "Levenshtein Distance".

To build some intuition, we should be able to notice that recursion is possible.
Since there are three choices(insert, delete, replace), the run-time would equate to 3^(max(length of word1, length of word2).
Because there is a recursion solution, lets come up with a dynamic programming solution instead.

A 2d array should come to mind, columns as word1, rows as word2 for each letter of each word.
Besides the three operations, there are also two other choices, whether the letter from each word matches.
If the letters match or not, then what we care about is the previous minimum operations.
Using the 2d array, we can figure out the previous minimum operations.
For any given dp element, the left, top and top-left values are what we care about.

```
Columns = word1
Rows = word2

Insert:
   '' a b
''  0 1 2
a   1 0 1
b   2 1 0
c   3 2 1

Delete:
   '' a b c
''  0 1 2 3
a   1 0 1 2
b   2 1 0 1

Replace:
   '' a b c
''  0 1 2 3
a   1 0 1 2
b   2 1 0 1
d   3 2 1 1
```

So for any given dp element, dp[i][j] = 1 + min(dp[i-1][j-1], d[i-1][j], dp[i][j-1]).
The only important thing to consider is when the letters match.
For that scenario, dp[i-1][j-1] + 1 does not apply, it doesn't need any operations done for that dp[i][j].

```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:

        def create_dp():
            dp = [[0] * (len(word1) + 1) for _ in range(len(word2) + 1)]
            for idx in range(len(word1) + 1):
                dp[0][idx] = idx
            for idx in range(len(word2) + 1):
                dp[idx][0] = idx
            return dp

        dp = create_dp()
        for col_idx, ch1 in enumerate(word1, 1):
            for row_idx, ch2 in enumerate(word2, 1):
                top_left = dp[row_idx-1][col_idx-1]
                if ch1 == ch2:
                    top_left -= 1
                dp[row_idx][col_idx] = 1 + min(top_left, # top left corner
                                           dp[row_idx][col_idx-1], # left
                                           dp[row_idx-1][col_idx]) # above
        return dp[-1][-1]
```
