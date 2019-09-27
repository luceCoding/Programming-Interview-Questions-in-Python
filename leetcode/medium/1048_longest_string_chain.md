# 1048. Longest String Chain

## Memoization with Recursion Solution

- Runtime: O(NC) + O(C^C)
- Space: O(C)
- N = Number of words in list
- C = Longest character word

We can come up with a recursive solution quite easily by removing each letter and calling the next recursion function on each newly formed word.
However, this would equate to a run-time of O(C^C), since we have to do this N times, and each time we have to delete each character O(NC), it would then be O(NC) + O(C^C).

We can improve our run-time by using memoization, instead of redoing checks, we just check once for each path and save that result.
So if we can only build a chain of 3 with 'abcd', if given 'abcde', when it comes to removing 'e', we don't need to check each character of 'abcd' again, instead just return 3.

```
class Solution:
    def longestStrChain(self, words: List[str]) -> int:

        def chain_helper(word):
            if word in memo:
                return memo[word]
            longest_chain = 1
            for idx in range(len(word)):
                new_word = word[:idx] + word[idx+1:]
                if new_word in word_set:
                    longest_chain = max(chain_helper(new_word)+1, longest_chain)
            memo[word] = longest_chain
            return longest_chain

        memo = dict()
        word_set = set(words)
        for word in words:
            chain_helper(word)
        return max(memo.values(), default=0)
```
