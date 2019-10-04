# 1032. Stream of Characters

## Trie Solution
- Run-time: O(N * Q)
- Space: O(C)
- N = Number of characters in stream
- Q = Number of queries
- C = Number of characters in word list

When dealing with single characters, its worth considering the trie data structure.
If we created a trie of the word list, we can figure out existence of words from the stream up to the recent query.
However, you may notice that if we built the trie structure beginning from left to right, it would result in a slower run-time.
This is because the new letter from the stream is at the right-most position while the trie structure starts at the left-most letter of each word in the word list.
Instead of building it from the left to right, we can build the trie structure in reverse.
That means, both the trie and the stream of letters would be traversed from the right to the left together.
This can optimize for most outputs, however, there are still worst case inputs like a given word list of A's and a query of A's.
This would result in an O(N * Q) run-time. 

```
from collections import defaultdict

class StreamChecker:

    def __init__(self, words: List[str]):
        self.root = TrieNode.create_tries(words)
        self.stream = list()

    def query(self, letter: str) -> bool:
        self.stream.append(letter)
        curr = self.root
        for ch in reversed(self.stream):
            if ch not in curr.next:
                return False
            curr = curr.next[ch]
            if curr.is_word:
                return True
        return False

class TrieNode(object):

    def __init__(self):
        self.next = defaultdict(TrieNode)
        self.is_word = False

    def __repr__(self):
        return '{} {}'.format([ch for ch in self.next], self.is_word)

    @staticmethod
    def create_tries(words):
        root = TrieNode()
        for word in words:
            curr = root
            for ch in reversed(word):
                curr = curr.next[ch]
            curr.is_word = True
        return root
```
