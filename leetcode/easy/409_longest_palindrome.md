# 409. Longest Palindrome

## Best Solution
- Runtime: O(N)
- Space: O(N)
- N = Number of characters in string

The question asks for the longest length, not the exact string that needs to be built, this greatly simplifies the question.
Firstly, think about how a palindrome would be built. 

Some questions to ask are:
1. What character or characters should I choose as the starting middle?
2. What happens if there are no odd number of characters, just all even?
3. What happens if there are multiple odd number of characters?

Solution Steps:
1. Count the occurances of every character in the string via. a dictionary. 
2. Then for each occurance, only count up to the highest even number, both for odd and even occurances.
3. During this, we keep track if we found an occurance of only one and if we found an odd occurance greater than one.

This only one and greater than one is for when we were selecting the middle character to build the palindrome with.
If there exists a character that only exists once, that means we can use that as the middle character of the palindrome, all other characters that occur once will be ignored.
Similarly, if no character has an occurance of one, since we only count the highest even occurance for every occurance, we can simply add one to the length if there was an odd occurance greater than one.

```
from collections import Counter

class Solution:
    def longestPalindrome(self, s: str) -> int:
        ch_to_count = Counter(s)
        longest_length = 0
        only_one = odd_greater_than_one = False
        for count in ch_to_count.values():
            if count % 2 == 0: # even
                longest_length += count
            else: # odd
                if count == 1:
                    only_one = True
                else:
                    odd_greater_than_one = True
                longest_length += count-1
        if only_one:
            longest_length += 1
        elif odd_greater_than_one:
            longest_length += 1
        return longest_length
```
