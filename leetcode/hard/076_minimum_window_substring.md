# 76. Minimum Window Substring

## Two Pointer with Map Solution
- Runtime: O(S * T) but O(S * (S+T)) due to string slicing
- Space: O(S)
- S = Number of characters in string S
- T = Number of unique characters in string T

So the definition of a result requires the substring to contain all characters in T. It then wants the smallest substring.
With that, we have to begin the solution by finding the first instance of that substring.
Once we have that, we need to basically prune it, remove the extra characters from this substring to get the minimum substring.
Finally, we keep repeating this until we reach the end.
We can traverse the string via. two pointers, a left and right iterator.
Right is bound within the given string S and left is bound within the substring created by the right pointer.
This is an example of a sliding window.

To figure out if we have all the characters in this substring, we would have to count T via. a dictionary.
This is our known count that is required for each substring.
Then we have to keep a dynamic counter, decrementing and incrementing character counts as we move across the string.
With this, after we moved the right pointer to the right, we can use these two dictionaries to check if this is a substring that meets the requirements.
If so, we can then try pruning with the left pointer all the way to the right pointer or if it doesn't meet the requirement.

For the run-time, worst case, we will be visiting each character twice.

You may think that the run-time for this is exponential, especially when we are checking the two dictionaries.
However, don't be mistaken, the comparison is actually a constant T run-time, it doesn't change based on S, but rather on T.
There is one slight problem, python's implementation of string concatention is actually **O(N)**.
When the question wants the actual substring and not a count, even using a deque will not solve this problem.
So this implementation is technically **O(S * (S+T))** due to python.

When implementing these type of two pointer questions.
I recommend to **avoid using indexes as much as possible and use iterators**.
It is very easy to get a one off error doing these and within a 30 minute timeframe, it is very risky.
Just talk about using indexes instead and you will be fine.

```
from collections import defaultdict
from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        all_ch_counts = Counter(t)
        ch_to_n_counts = defaultdict(int)
        str_builder, min_substr, found = '', s, False
        for right_ch in s:
            ch_to_n_counts[right_ch] += 1
            str_builder += right_ch
            if chars_occur_ge(ch_to_n_counts, all_ch_counts):
                for left_ch in str_builder:
                    if chars_occur_ge(ch_to_n_counts, all_ch_counts):
                        found = True
                        if len(str_builder) < len(min_substr):
                            min_substr = str_builder
                    else:
                        break
                    ch_to_n_counts[left_ch] -= 1
                    str_builder = str_builder[1:]
        return min_substr if found else ''
        
def chars_occur_ge(ch_to_n_counts, all_ch_counts):
    for ch, count in all_ch_counts.items():
        if ch not in all_ch_counts or ch_to_n_counts[ch] < count:
            return False
    return True
```

## Two Pointer with Map Solution (Optimized)
- Runtime: O(S)
- Space: O(S)
- S = Number of characters in string S
- T = Number of unique characters in string T

We can further improve the solution by optimizing the way we check if its a valid substring.
We can still use a dictionary to count the occurances, but we can also keep a separate count for the unique characters in T.
This will represent the number of unique valid characters of T.

If T = 'abcc', then there are 3 keys in the dictionary.
When ever we increment a key in the dictionary, we can then compare the dictionary T's count with dictionary S's count. 
If S's count equals exactly what T's count is, then we just got one of the 3 keys validated. 
If we decrement, and S's count is T's count-1, then we just unvalidated one of those keys.

This will remove the need to traverse all the keys whenever we need to revalidate the substring.
Hence, this validation will run at O(1). String slicing will still slow us down however.

```
from collections import defaultdict
from collections import Counter

class Solution:
    def minWindow(self, s: str, t: str) -> str:
        char_counter = MinCharacterCounter(t)
        str_builder, min_substr, found = '', s, False
        for right_ch in s:
            char_counter.increment(right_ch)
            str_builder += right_ch
            if char_counter.is_valid:
                for left_ch in str_builder:
                    if char_counter.is_valid:
                        found = True
                        if len(str_builder) < len(min_substr):
                            min_substr = str_builder
                    else:
                        break
                    char_counter.decrement(left_ch)
                    str_builder = str_builder[1:]
        return min_substr if found else ''
        
class MinCharacterCounter:
    def __init__(self, source_str):
        self._ch_to_n_counts = defaultdict(int)
        self._source_counts = Counter(source_str)
        self._n_valid_chars = 0

    def increment(self, char):
        self._ch_to_n_counts[char] += 1
        if char in self._source_counts and self._ch_to_n_counts[char] == self._source_counts[char]:
            self._n_valid_chars += 1
        
    def decrement(self, char):
        self._ch_to_n_counts[char] -= 1
        if char in self._source_counts and self._ch_to_n_counts[char] == self._source_counts[char]-1:
            self._n_valid_chars -= 1
    
    @property
    def is_valid(self):
        return self._n_valid_chars == len(self._source_counts.keys())
```
