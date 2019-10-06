# 212. Word Search II

## Trie + DFS Solution
- Run-time: O((R \* C)^2)
- Space: O(W)
- R = Number of Rows
- C = Number of Columns

In order to figure out if a word exists in the list of words, it is require to do some sort of traversal on the board, generally DFS will do here.
Secondly, by using a trie, we can traverse the board and trie together one character at a time.

Each DFS will be for the worst case, traversing the longest word in the word list.
For example, a board full of a's and word list of different lengths of a's. The longest word could end up being as long as all the elements on the board.
So the run-time will total to O((R \* C)^2).

```
from collections import defaultdict

class Solution:
    def findWords(self, board: List[List[str]], words: List[str]) -> List[str]:

        def dfs(trie, r, c, word=list(), visited=set()):
            if (r, c) in visited or board[r][c] not in trie.next:
                return
            visited.add((r, c))
            word.append(board[r][c])
            trie = trie.next[board[r][c]]
            if trie.is_word:
                results.append(''.join(word))
                trie.is_word = False # avoid duplicates
            for _r, _c in get_neighbors(r, c):
                dfs(trie, _r, _c, word, visited)
            word.pop()
            visited.remove((r, c))

        def get_neighbors(r, c):
            dirs = [(1, 0), (0, 1), (-1, 0), (0, -1)]
            for _r, _c in dirs:
                _r += r
                _c += c
                if 0 <= _r < len(board) and 0 <= _c < len(board[0]):
                    yield (_r, _c)

        root = TrieNode.create_tries(words)
        results = list()
        for r, row in enumerate(board):
            for c in range(len(row)):
                dfs(root, r, c)
        return results

class TrieNode(object):

    def __init__(self):
        self.next = defaultdict(TrieNode)
        self.is_word = False

    def __repr__(self):
        return 'Next: {}, IsWord: {}'.format(self.next.keys(), self.is_word)

    @staticmethod
    def create_tries(words):
        root = TrieNode()
        for word in words:
            curr = root
            for ch in word:
                curr = curr.next[ch]
            curr.is_word = True
        return root
```
