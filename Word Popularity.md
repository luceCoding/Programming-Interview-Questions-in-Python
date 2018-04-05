Given an integer W that represents number of words,
Write functions insert(word) and getMostPopularWord() such that getMostPopularWord() will always return the most popular word in the last W number of words.
The goal is to make insert(word) as efficient as possible.

For example:
let W = 3
insert("A")
insert("A")
getMostPopularWord() => "A"
insert("B")
getMostPopularWord() => "A"
insert("B")
getMostPopularWord() => "B" // since the first inserted "A" is out of the scope of the last 3 words

'''
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
'''
