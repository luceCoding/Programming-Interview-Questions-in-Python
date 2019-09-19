# 76. Minimum Window Substring

## Sliding Window with Dictionary Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of characters in S

Lets focus on how to figure out if a sub-string has all characters of T.
You can do this with a dictionary counter, keeping occurances of each character of T.
To avoid checking if each character in the dictionary is less than or equal to zero occurances, we can keep a separate variable as the remmaining characters needed to be found.

Next is the idea of a sliding window, if we iterate from left to right, we can find all the sub-strings containing T using the above soluiton.
Once that sub-string is found, then its the matter of decrementing the left most character of the sub-string until we need to find another character.
With that, we have to decrement the dictionary and the number of remaining characters accordingly.

```
from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        char_counter = Counter(t)
        left_idx, n_chars_needed = 0, len(t)
        result = (-1, -1) # left and right result indexes
        
        for right_idx, ch in enumerate(s):
            if ch in char_counter:
                char_counter[ch] -= 1
                if char_counter[ch] >= 0:
                    n_chars_needed -= 1
            
            while n_chars_needed == 0:
                if result[0] == -1 or result[1]-result[0] > right_idx-left_idx:
                    result = (left_idx, right_idx)
                left_ch = s[left_idx]
                if left_ch in char_counter:
                    char_counter[left_ch] += 1
                    if char_counter[left_ch] == 1:
                        n_chars_needed += 1
                left_idx += 1
            
        return s[result[0]:result[1]+1] if result[0] != -1 else ''
```
