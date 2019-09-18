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
class Solution(object):
    def minWindow(self, S, T):
        
        def trim(start, end):
            t_idx = len(T)-1
            for idx, ch in reversed(list(enumerate(S[start:end+1], start))):
                if ch == T[t_idx]:
                    t_idx -= 1
                if t_idx < 0:
                    return idx
            return start
        
        min_substr = S + ' '
        t_idx = start_idx = 0
        for idx, ch in enumerate(S):
            if ch == T[t_idx]:
                t_idx += 1
            if t_idx >= len(T):
                start_idx = trim(start_idx, idx)
                min_substr = min(min_substr, S[start_idx:idx+1], key=lambda x: len(x))
                t_idx = len(T)-1
        return min_substr if len(min_substr) < len(S) else ''
```
