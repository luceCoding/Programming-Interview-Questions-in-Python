# 1032. Stream of Characters

## Trie Solution
- Run-time: O(C * Q)
- Space: O(C) + O(N)
- N = Number of characters in stream
- Q = Number of queries
- C = Number of characters in word list

When dealing with single characters, its worth considering the trie data structure.
If we created a trie of the word list, we can figure out existence of words from the stream up to the recent query.
However, you may notice that if we built the trie structure beginning from left to right, it would result in a slower run-time.
This is because the new letter from the stream is at the right-most position while the trie structure starts at the left-most letter of each word in the word list.
Instead of building it from the left to right, we can build the trie structure in reverse.
That means, both the trie and the stream of letters would be traversed from the right to the left together.

Each query will result in O(C) run-time, since we have N queries, this will total to O(C * Q).
However, there are still worst case inputs like a given word list of ['baaaaaaaaaaaaaa'] and a query of a's.
But for a general case, since the trie is built in reverse, if the most recent letter in the stream doesn't exist in the root, it will be less than O(C) for each query.

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
        return '{} {}'.format(self.next.keys(), self.is_word)

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
