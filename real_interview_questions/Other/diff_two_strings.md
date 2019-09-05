# Diff Between Two Strings

Given two strings of uppercase letters source and target, list (in string form) a sequence of edits to convert from source to target that uses the least edits possible.

For example, with strings source = "ABCDEFG", and target = "ABDFFGH" we might return: ["A", "B", "-C", "D", "-E", "F", "+F", "G", "+H"

More formally, for each character C in source, we will either write the token C, which does not count as an edit; or write the token -C, which counts as an edit.

Additionally, between any token that we write, we may write +D where D is any letter, which counts as an edit.

At the end, when reading the tokens from left to right, and not including tokens prefixed with a minus-sign, the letters should spell out target (when ignoring plus-signs.)

In the example, the answer of A B -C D -E F +F G +H has total number of edits 4 (the minimum possible), and ignoring subtraction-tokens, spells out A, B, D, F, +F, G, +H which represents the string target.

If there are multiple answers, use the answer that favors removing from the source first.

Constraints:

[time limit] 5000ms

[input] string source
2 ≤ source.length ≤ 12

[input] string target
2 ≤ target.length ≤ 12

[output] array.string

# Solution

The trickiest part of all this is to build the intuition for how to handle the fork.
You should notice that there are only two possible ways when a char from target and source do not match.
Whether to delete S or add T.
We would have to traverse the entire solution space because we don't know if we can build a smaller list.
After that, its a simpler recursive call and returning the min list.

```
def diffBetweenTwoStrings(source, target):
  def diff_helper(s, t):
    if len(t) == 0 and len(s) > 0:
      return ['-' + ch for ch in s]
    elif len(t) > 0 and len(s) == 0:
      return ['+' + ch for ch in t]
    elif len(t) == 0 and len(s) == 0:
      return []
    if s[0] == t[0]:
      return [s[0]] + diff_helper(s[1:], t[1:])
    # s[0] != t[0]
    result1 = diff_helper(s[1:], t) # skip s, delete s
    result2 = diff_helper(s, t[1:]) # skip t, add t
    if len(result1) <= len(result2):
      return ['-' + s[0]] + result1
    else:
      return ['+' + t[0]] + result2
    
  return diff_helper(source, target)
```
