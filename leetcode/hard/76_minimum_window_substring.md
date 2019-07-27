# 76. Minimum Window Substring

## Two Pointer with Map Solution
- Runtime: O(S * T)
- Space: O(S)
- S = Number of characters in string S
- T = Number of unique characters in string T

So the definition of a result requires the substring to contain all characters in T. It then wants the smallest substring.
With that, we have to begin the solution by finding the first instance of that substring.
Once we have that, we need to basically prune it, remove the extra characters from this substring to get the minimum substring.
Finally, we keep repeating this until we reach the end.
We can traverse the string via. two pointers, a left and right iterator.
Right is bound within the given string S and left is bound within the substring created by the right pointer.

To figure out if we have all the characters in this substring, we would have to count T via. a dictionary.
This is our known count that is required for each substring.
Then we have to keep a dynamic counter, decrementing and incrementing character counts as we move across the string.
With this, after we moved the right pointer to the right, we can use these two dictionaries to check if this is a substring that meets the requirements.
If so, we can then try pruning with the left pointer all the way to the right pointer or if it doesn't meet the requirement.

You may think that the run-time for this is exponential, especially when we are checking the two dictionaries.
However, don't be mistaken, the comparison is actually a constant T run-time, it doesn't change based on S, but rather on T.
There is one slight problem, python's implementation of string concatention is actually O(N).
When the question wants the actual substring and not a count, even using a deque will not solve this problem.
So this implementation is technically O(S * (S+T)) due to python, but in other languages

When implementing these type of two pointer questions.
I recommend to avoid using indexes as much as possible and use iterators.
It is very easy to get a one off error doing these and within a 30 minute timeframe, it is very risky.

```
from collections import defaultdict
from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        all_ch_counts = Counter(t)
        ch_to_n_counts = defaultdict(int)
        str_builder, min_substr, found = '', s, False
        for curr_right in s:
            ch_to_n_counts[curr_right] += 1
            str_builder += curr_right
            if chars_occur_ge(ch_to_n_counts, all_ch_counts):
                for curr_left in str_builder:
                    if chars_occur_ge(ch_to_n_counts, all_ch_counts):
                        found = True
                        if len(str_builder) < len(min_substr):
                            min_substr = str_builder
                    else:
                        break
                    ch_to_n_counts[curr_left] -= 1
                    str_builder = str_builder[1:]
        return min_substr if found else ''
        
def chars_occur_ge(ch_to_n_counts, all_ch_counts):
    for ch, count in all_ch_counts.items():
        if ch not in all_ch_counts or ch_to_n_counts[ch] < count:
            return False
    return True
```
