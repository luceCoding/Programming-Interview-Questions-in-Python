# EXPLAINATION
This is a very interesting problem.
Whenever you heard the word 'shortest', BFS pops up in my head. 
However, at first you may not noticed that is actually a graph problem. 
If you first create a graph structure where two words are related based on being one distance apart. You can then combine BFS to find the shortest distance to the desired string.

I have to mention the method "create_undirected_adj_list". The run-time is near constant, it is based on the longest string length. As the amount of words grow, the "create_undirected_adj_list" method will stay constant.
While the big O of the entire solutioni is O(n), N being all the nodes in the graph, which means the amount of words. We only visit each node only once, in the worst case.

# SOLUTION
```
class Solution(object):
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        wordList.append(beginWord)
        adj_list = self.create_undirected_adj_list(wordList)
        return self.bfs_get_min_distance(beginWord, endWord, adj_list)
    
    def bfs_get_min_distance(self, begin_word, end_word, adj_list):
        bfs_queue = collections.deque()
        bfs_queue.appendleft(begin_word)
        distance = 0
        visited_words = set(begin_word)
        while len(bfs_queue) != 0:
            n_pops = len(bfs_queue)
            distance += 1
            while n_pops > 0:
                curr_word = bfs_queue.pop()
                if curr_word == end_word:
                    return distance
                next_words = adj_list[curr_word]
                for next_word in next_words:
                    if next_word not in visited_words:
                        visited_words.add(next_word)
                        bfs_queue.appendleft(next_word)
                n_pops -= 1
        return 0
    
    def create_undirected_adj_list(self, word_list):
        word_set = set(word_list)
        undirected_adj_list = dict()
        for index, target_word in enumerate(word_list):
            sub_word_list = word_list[index+1:]
            if target_word not in undirected_adj_list:
                undirected_adj_list[target_word] = list()
            one_edit_off_words = self.get_one_edit_off_words_from(target_word, word_set)
            for one_off_word in one_edit_off_words:
                if one_off_word not in undirected_adj_list:
                    undirected_adj_list[one_off_word] = list()
                undirected_adj_list[target_word].append(one_off_word)
                undirected_adj_list[one_off_word].append(target_word)
        return undirected_adj_list
        
    def get_one_edit_off_words_from(self, target_word, word_set):
        result = list()
        for index in range(0, len(target_word)):
            for ch in "qwertyuiopasdfghjklzxcvbnm":
                test_word = target_word[:index] + ch + target_word[index+1:]
                if test_word in word_set:
                    result.append(test_word)
        return result
```
