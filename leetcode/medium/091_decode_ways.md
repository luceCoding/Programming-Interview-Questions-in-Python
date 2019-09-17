# 91. Decode Ways

## Brute Force Recursion Solution

- Runtime: O(2^N)
- Space: O(N)
- N = Number of elements in list

We should be able to recognize that we can basically ask two questions, for each character, can we decode this character or if we can decode this character with the previous character?
With this, you can build a recursion function to solve this solution.

```
class Solution(object):
    def numDecodings(self, s):
        
        def decode_helper(s):
            if len(s) == 0:
                return 1
            n_ways = 0
            if int(s[0]) != 0:
                n_ways += decode_helper(s[1:])
            if len(s) >= 2 and 10 <= int(s[:2]) <= 26:
                n_ways += decode_helper(s[2:])
            return n_ways
            
        if len(s) == 0:
            return 0
        return decode_helper(s)
```

## Dynamic Programming Solution

- Runtime: O(N)
- Space: O(N)
- N = Number of elements in list

Since we can recognize that there exists a recursion function, this can tell us that there is also a dynamic programming solution too.
We already know the two sub-problems, whether to decode current character or current + previous character.

With any dynamic programming solution, we need some sort of array.
We can see that a 2d array can be used to represent number of ways to decode a character.
With this, we can store the previous calculated numbers and check them as we go left to right in the string.

So given s='123', dp[0] will represent an empty string, dp[1] will represent '1', dp[2] will represent '2' and so forth.
We can then deduct that at any given character, dp[n] = dp[n-1] + dp[n-2].

```
class Solution(object):
    def numDecodings(self, s):
        if len(s) == 0:
            return 0
        s = '0' + s # represent empty string
        n_ways = [0] * (len(s))
        n_ways[0] = 1 # set empty string
        for idx, ch in enumerate(s[1:], 1):
            if int(ch) != 0:
                n_ways[idx] += n_ways[idx-1]
            if 10 <= int(s[idx-1:idx+1]) <= 26:
                n_ways[idx] += n_ways[idx-2]
        return n_ways[-1]
```

## Optimal Solution

- Runtime: O(N)
- Space: O(1)
- N = Number of elements in list

You can further optimize space by just keeping just two variables to represent the previous character and the one before that previous character. We don't need the entire N array, once we use up the past DP elements, they will no longer be used anymore.

```
class Solution(object):
    def numDecodings(self, s):
        if len(s) == 0:
            return 0
        s = '0' + s # represent empty string
        prev_prev, prev = 0, 1
        for idx, ch in enumerate(s[1:], 1):
            result = 0
            if int(ch) != 0:
                result += prev
            if 10 <= int(s[idx-1:idx+1]) <= 26:
                result += prev_prev
            prev_prev = prev
            prev = result
        return prev
```
