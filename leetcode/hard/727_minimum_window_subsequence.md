# 727. Minimum Window Subsequence

## Multiple Pointer Solution
- Runtime: O(N^2)
- Space: O(1)
- N = Number of characters in string

By keeping a pointer on T and a pointer to the start of the sub-string.
We can iterate each character in the string and compare it to T to keep the ordering.
Once we have found all of T, we now have a sub-string that has all of T in it and has the ordering we want.
However, we need to trim it to make it the minimum subsequence, so we would have to traverse the sub-string in reverse order.
The trim stage would need to compare the sub-string and T both in reverse order to find the miniumum subsequence.

The worst case can be when we have an input like S = 'aaaaaa', T = 'aa'.
We would have to attempt a trim stage for every character, making it O(N^2).

Some edge cases to consider:
```
S = 'abcabdcd'
T = 'abcd'
Expected Output = 'abdcd'
```

```
class Solution:
    def minWindow(self, S: str, T: str) -> str:
        
        def trim(start):
            t_idx = len(T)-1
            while t_idx >= 0:
                if S[start] == T[t_idx]:
                    t_idx -= 1
                if t_idx < 0:
                    return start
                start -= 1
            return 0
        
        window = S + ' '
        t_idx = s_idx = 0
        while s_idx < len(S):
            if S[s_idx] == T[t_idx]:
                t_idx += 1
                if t_idx == len(T):
                    end = s_idx
                    s_idx = trim(s_idx)
                    t_idx = 0
                    window = min(window, S[s_idx:end+1], key=lambda x: len(x))
            s_idx += 1
        return window if len(window) < len(S) else ''
```
