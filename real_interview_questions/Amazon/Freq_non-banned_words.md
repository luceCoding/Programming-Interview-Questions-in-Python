# QUESTION
Given a paragraph and a list of banned words, return the most frequent word that is not in the list of banned words. It is guaranteed there is at least one word that isn't banned, and that the answer is unique.
Words in the list of banned words are given in lowercase, and free of punctuation. Words in the paragraph are not case sensitive. The answer is in lowercase.
```
Example:
 Input: 
 paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
 banned = ["hit"]
 Output: "ball"
 Explanation: 
 "hit" occurs 3 times, but it is a banned word.
 "ball" occurs twice (and no other word does), so it is the most frequent non-banned word in the paragraph. 
 Note that words in the paragraph are not case sensitive,
 that punctuation is ignored (even if adjacent to words, such as "ball,"), 
 				and that "hit" isn't the answer even though it occurs more because it is banned.
```

# SOLUTION
```
def get_most_freq_word(paragraph, banned_words):
    if paragraph is None:
        return ''
    if banned_words is None:
        banned_words = list()
    tokens = paragraph.split()
    tokens = sanitize(tokens)
    banned_words_set = set(banned_words)
    word_to_occur_pos_hash = create_occur_pos_hash(tokens, banned_words_set)
    return find_most_freq_word_in(word_to_occur_pos_hash)

def sanitize(tokens):
    result = list()
    for token in tokens:
        full_word = list()
        for letter in token:
            letter = letter.lower()
            if letter.isalpha():
                full_word.append(letter)
        if len(full_word) > 0:
            full_word = ''.join(full_word)
            result.append(full_word)
    return result

def create_occur_pos_hash(tokens, banned_words_set):
    result = dict()
    for pos, word in enumerate(tokens):
        if word not in banned_words_set:
            if word not in result:
                result[word] = (0,0)
            occurance = result[word][0]
            result[word] = (occurance+1, pos)
    return result

def find_most_freq_word_in(wtop_hash):
    result, global_occurance, global_position = '', -1, -1
    for word, value in wtop_hash.items():
        local_occurance = value[0]
        local_position = value[1]
        if local_occurance >= global_occurance:
            if local_position > global_position:
                result = word
                global_occurance = local_occurance
                global_position = local_position
    return result

paragraph = "Bob hit a ball, the hit BALL flew far after it was hit."
banned = ["hit"]
assert(get_most_freq_word(paragraph, banned) == 'ball')

paragraph = "    a1 b# 0 $ a b c (    "
banned = ["b"]
assert(get_most_freq_word(paragraph, banned) == 'a')

paragraph = "    a1 b# 0 $ a b c (    "
banned = [""]
assert(get_most_freq_word(paragraph, banned) == 'b')

paragraph = ""
banned = ["b"]
assert(get_most_freq_word(paragraph, banned) == '')

paragraph = None
banned = ["b"]
assert(get_most_freq_word(paragraph, banned) == '')

paragraph = "a b a b"
banned = None
assert(get_most_freq_word(paragraph, banned) == 'b')

```
