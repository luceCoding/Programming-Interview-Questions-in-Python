# 727. Minimum Window Subsequence

## Multiple Pointer Solution
- Runtime: O(S^2)
- Space: O(1)
- S = Number of characters in S

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

## Dynamic Programming Solution
- Runtime: O(S\*T)
- Space: O(T)
- S = Number of characters in S
- T = Number of characters in T

The intution for this solution comes from using a 1d array as a way to build a pseudo linked chain.
If we have a 1d array of size T, each element representing the characters of T.
At any given T[n], will represent a valid possible contiguous sub-string pointing to the first possible index of T[0].
If a character of S exists in T, then we will set appropriate element of DP to the previous DP element, DP[n] = DP[n-1].
This builds the chain-like property to fulfill the ordering requirement of the sub-string.

The only exception to this rule is if we find that the given character of S is the first character of T (T[0]), then we should set all duplicate characters of T first before changing DP[0] to the current index. This will start a new possible chain link that could ripple across the other DP elements if the right ordering occurs.

Worst case is when T and S are the same duplicate set of characters. Like S = 'aaaaaaa' and T = 'aa'.
This would make us set all DP elements of T for each S.

```
from collections import defaultdict

class Solution(object):
    def minWindow(self, S, T):
        dp = [-1] * len(T)
        ch_to_t_idx = defaultdict(list)
        min_substr = S + ' '
        for idx, t in enumerate(T):
            ch_to_t_idx[t].append(idx)
        for idx, ch in enumerate(S):
            if ch in ch_to_t_idx:
                for t_idx in ch_to_t_idx[ch][::-1]:
                    if t_idx == 0: # first letter of T
                        dp[t_idx] = idx
                    else:
                        dp[t_idx] = dp[t_idx-1]
                    if t_idx == len(T)-1 and dp[t_idx] != -1: # last letter of T, save min
                        min_substr = min(min_substr, S[dp[t_idx]:idx+1], key=lambda x: len(x))
        return min_substr if len(min_substr) < len(S) else ''
```
