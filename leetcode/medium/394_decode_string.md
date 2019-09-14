# 394. Decode String

## Iterative Stack Solution

- Runtime: O(N)
- Space: O(N)
- N = Number of elements in array

This was actually a Google onsite interview question given to me once.

Given "3[a]2[bc]". This is how the stack should look like going from left to right.

```
['3']
['3', '[']
['3', '[', 'a']
['aaa']
['aaa', '2']
['aaa', '2', '[']
['aaa', '2', '[', 'b']
['aaa', '2', '[', 'b', 'c']
['aaa', 'bcbc']
```

Assuming you understand the stack, we can just go left to right and store each character into the stack.
When we get a ']', we have to concatenate or calculate the sub-string.
The reason we need to store the '[' is to know when to stop and gather the number for each string.
The '[' will also help when we have k[encoded_string] patterns within one another.

For example: "3[a2[c]]"

```
['3']
['3', '[']
['3', '[', 'a']
['3', '[', 'a', '2']
['3', '[', 'a', '2', '[']
['3', '[', 'a', '2', '[', 'c']
['3', '[', 'a', 'cc']
['accaccacc']
```

As in recursion, we need to concatenate each sub-answer together in the end.
We can have a multiple sub-strings in our stack.

```
class Solution:
    def decodeString(self, s: str) -> str:
        stack = list()
        for ch in s:
            if ch == ']':
                string = ''
                while len(stack) != 0 and stack[-1] != '[':
                    string = stack.pop() + string
                stack.pop() # pop '['
                num = ''
                while len(stack) != 0 and stack[-1].isdigit():
                    num = stack.pop() + num
                if num == '':
                    num = 1
                stack.append(string * int(num))
            else:
                stack.append(ch)
        return ''.join(stack)
```
