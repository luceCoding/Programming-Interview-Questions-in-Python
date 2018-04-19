# QUESTION
Given an integer W that represents number of words and unlimited space,
Write functions insert(word) and getMostPopularWord() such that getMostPopularWord() will always return the most popular word in the last W number of words.
Create two solutions that will optimize run-time for either function while sacrificing run-time for the other. 

For example:
```
let W = 2
insert("A")
getMostPopularWord() => "A"
insert("B")
getMostPopularWord() => "B"
```

```
let W = 3
insert("A")
insert("A")
getMostPopularWord() => "A"
insert("B")
getMostPopularWord() => "A"
insert("B")
getMostPopularWord() => "B" // since the first inserted "A" is out of the scope of the last 3 words
```

# SOLUTION 1
```
from collections import deque

class solution(object):
    def __init__(self, n_words_to_hold):
        self.n_words_to_hold = n_words_to_hold
        self.word_queue = deque()
        self.word_to_sorted_hash = dict()
        self.sorted_popularity = list()
    
    def insert(self, word):
        if len(self.word_queue) == self.n_words_to_hold:
            oldest_word = self.word_queue.pop()
            self.decrement_word(oldest_word)
        self.word_queue.appendleft(word)
        self.increment_word(word)
        self.sorted_popularity.sort(custom_compare)
        print 'sorted {}: {}'.format(word, self.sorted_popularity)
        print self.word_queue
        
    def getMostPopularWord(self):
        if len(self.sorted_popularity) != 0:
            return self.sorted_popularity[0][1]
        return ''
    
    def increment_word(self, new_word):
        popularity = 1
        if new_word in self.word_to_sorted_hash:
            node = self.word_to_sorted_hash[new_word]
            popularity += node[0]
            self.sorted_popularity.remove(node)
        new_node = [popularity, new_word]
        self.word_to_sorted_hash[new_word] = new_node
        # Trick to insert at beginning so that the sort will place priority for elements in the beginning
        # There might be a better way than this
        self.sorted_popularity.insert(0,new_node)
        
    def decrement_word(self, old_word):
        old_node = self.word_to_sorted_hash[old_word]
        old_node[0] -= 1
    
def custom_compare(a, b):
    if b[0] >= a[0]:
        return 1
    return -1

test = solution(2)
test.insert('A')
myOtestbj.insert('B')
test.insert('A')
print test.getMostPopularWord()
```

# SOLUTION 2
```
from collections import deque
import sys

class popular_words(object):
    def __init__(self, n_words_to_hold):
        self.n_words_to_hold = n_words_to_hold
        self.word_queue = deque()
        self.word_counter = dict()
        self.word_to_popularity = dict()
        self.counter = -sys.maxint - 1
        
    def insert(self, new_word):
        if self.n_words_to_hold == 0:
            return
        if new_word in self.word_to_popularity:
            self.word_to_popularity[new_word] += 1
        else:
            self.word_to_popularity[new_word] = 1
        if len(self.word_queue) == self.n_words_to_hold:
            oldest_word = self.word_queue.pop()
            self.word_to_popularity[oldest_word] -= 1
        self.word_queue.appendleft(new_word)
        self.counter += 1
        self.word_counter[new_word] = self.counter
        
    def getMostPopularWord(self):
        most_popular_num = 0
        most_popular_word = ''
        lastest_counter = -sys.maxint - 1
        for word, popularity in self.word_to_popularity.items():
            if popularity > most_popular_num:
                most_popular_num = popularity
                lastest_counter = self.word_counter[word]
                most_popular_word = word
            elif popularity == most_popular_num:
                if self.word_counter[word] >= lastest_counter:
                    lastest_counter = self.word_counter[word]
                    most_popular_word = word
        return most_popular_word
    
test = popular_words(4)
test.insert('A')
test.insert('B')
print test.getMostPopularWord()
test.insert('A')
print test.getMostPopularWord()
test.insert('C')
print test.getMostPopularWord()
```

# FOLLOW UP QUESTION
Can you get both functions to run at O(1) most of the time?
