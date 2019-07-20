# 647. Palindromic Substrings

## Solution
- Runtime: O(N^2)
- Space: O(1)
- N = Number of elements in array

For every character, treat it as the center, attempt to expand the center until we cannot create another palindrome.
Remember, you can create a palindrome starting with one character or two characters. 
So you would need to select two centers per character in the string.

```
class Solution:
    def countSubstrings(self, s: str) -> int:
        n_palindromes = 0
        for index in range(0, len(s)):
            n_palindromes += self.get_n_expanded_palindromes(left=index, right=index, string=s)
            n_palindromes += self.get_n_expanded_palindromes(left=index, right=index+1, string=s)
        return n_palindromes
    
    def get_n_expanded_palindromes(self, left, right, string):
        n_palindromes = 0
        while left >= 0 \
              and right < len(string) \
              and string[left] == string[right]:
            n_palindromes += 1
            left, right = left-1, right+1
        return n_palindromes
```
