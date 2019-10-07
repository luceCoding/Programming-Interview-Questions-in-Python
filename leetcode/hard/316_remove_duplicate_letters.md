# 316. Remove Duplicate Letters

## Stack Solution
- Run-time: O(N)
- Space: O(1) or 26
- N = Number of characters in S

To gather the intuition for this solution lets look at a few examples.

```
Example 1:
abc -> abc

Example 2:
cba -> cba

Example 3:
aba -> ab

Example 4:
bab -> ab

Example 5:
xyzabczyx -> abczyx
```

Examples 1 and 2 don't really tell us much but combining them with examples 3 and 4 can.
Notice that example 3 doesn't care about the last letter 'a', while example 4 doesn't care about the first letter 'b'.

Instead of thinking about figuring out which letter to delete, we can think of this by building the word from scratch, from left to right.
With this frame of reference we can reword the question into finding the biggest lexicographic sorted word that doesn't contain duplicates.

From examples 3 and 4, we can then denote that if a letter isn't the last occurring letter, we can build a much larger word that is closer to being lexicographic.
So by using a monotonic stack, that is increasing by nature, we can achieve examples 1 and 3.
A set should be obvious to avoid adding duplicate values into the stack.
However, to achieve examples 2 an 5 with the monotonic stack, we have to add another invariant were we want last occurring letters.
We can skip letters we have already seen due to the fact that stack is monotonic, the letters in the stack are already in the best position so far, this is to achieve example 3.

In summary, we can build the word from left to right using an increasing monotonic stack.
When it comes to popping off the stack, we will continue to pop from the stack if the new letter is smaller than whats on top of the stack AND if whats on top of the stack isn't the last occurring letter.

```
class Solution:
    def removeDuplicateLetters(self, s: str) -> str:
        stack = list()
        seen = set()
        ch_to_last_idx = {ch: idx for idx, ch in enumerate(s)}
        result = ''
        for idx, ch in enumerate(s):
            if ch not in seen:
                while stack and ch < stack[-1] and idx < ch_to_last_idx[stack[-1]]:
                    seen.discard(stack.pop())
                seen.add(ch)
                stack.append(ch)
        return ''.join(stack)
```
