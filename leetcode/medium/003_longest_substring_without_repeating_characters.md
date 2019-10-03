# 3. Longest Substring Without Repeating Characters

## Sliding Window Solution
- Run-time: O(N) or 2N
- Space: O(N)
- N = Number of characters in string

Using a sliding window and a set, we can identify when we need to move the left side of the sliding window when a duplicate character is found as well as when to stop.

This solution is actually a two pass solution, due to the fact we have to remove elements on the left side from the set, hence, visiting each element twice.

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        seen = set()
        left_idx = longest_substr = 0
        for idx, ch in enumerate(s):
            while ch in seen and left_idx <= idx:
                seen.remove(s[left_idx])
                left_idx += 1
            seen.add(ch)
            longest_substr = max(longest_substr, len(seen))
        return longest_substr
```

## One Pass Solution
- Run-time: O(N)
- Space: O(N)
- N = Number of characters in string

To perform a one pass solution, we can use a dictionary where the key is the character and the value is its index.
Similar to the set, we can use the dictionary to check if we have a duplicate in our sliding window.
If that is true, we can immediately move the left side to the previous duplicated character's index + 1.

```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        seen = dict()
        left_idx = longest_substr = 0
        for idx, ch in enumerate(s):
            if ch in seen:
                left_idx = seen[ch] + 1
            seen[ch] = idx
            longest_substr = max(longest_substr, idx - left_idx + 1)
        return longest_substr
```
