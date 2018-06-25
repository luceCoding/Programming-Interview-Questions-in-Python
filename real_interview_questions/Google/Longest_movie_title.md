# QUESTION 
Return the longest movie title found by successively matching the last and first words in each title, joining to form new titles, given an array containing a list of movie titles.

```
For example: 
Input: ['OF MICE AND MEN', 'BLACK MASS', 'MEN IN BLACK"]
Output:  'OF MICE AND MEN IN BLACK MASS'

Explanation:
'OF MICE AND MEN' and 'MEN IN BLACK' join to form 'OF MICE AND MEN IN BLACK'. 
You could further join 'OF MICE AND MEN IN BLACK' wth 'BLACK MASS' to form 'OF MICE AND MEN IN BLACK MASS'.
```

# EXPLAINATION
This is another graph problem. Make an adjacent list for each title. You may need need a hash table to help you perform this, mainly a hash table with first word as the key and the title as the value.

Then create a method that goes through all the titles and performs a DFS. When using a DFS on a graph, you will need a stack/recursion to keep the current longest title found so far. For each neighbor, we will call a DFS and concat the result from that search to our title and check if that new result is better than our result found so far.

Also, we will need a different visited set for each DFS, this is to avoid cyclic searches and to avoid doing useless searches.
Say you have 'a b', 'b c', 'c d', 'd e'. If you begun your search at 'c d', you will get a result of 'c d d e'. 'c d' and 'd e' will be marked as visited but when you visit 'd e ' again, you should not begin a new DFS with this title. But for 'b c' or 'a b' its fine to start a new DFS search.

Worst case occurs when the titles you start your DFS search will always create the next smallest title. Therefore it would be O(n^2), where N is the number of titles.

You can avoid O(n^2) by using memoization. Therefore, you can get O(n). Also if there is a recursive solution, there will be a dp solution. You can also use your memoization as your global visited set in this case.

# SOLUTION
```import copy

def get_longest_title(titles):
    adj_list = create_adj_list(titles)
    longest_title = list()
    memo = dict()
    for title in adj_list.keys():
        if title not in memo:
            local_visit = set()
            local_result = visit_title(title,
                                        adj_list, 
                                        local_visit,
                                        memo)
            if len(local_result) > len(longest_title):
                longest_title = copy.deepcopy(local_result)
    return concat_title(adj_list, longest_title)

def concat_title(adj_list, titles):
    if len(titles) == 0:
        return ''
    result = list()
    result.append(titles[0])
    for index in range(1, len(titles)):
        title = titles[index]
        words_in_title = title.split()
        sub_title = words_in_title[1:]
        result += sub_title
    if is_cyclic(adj_list, titles):
        result = result[:-1]
    return ' '.join(result)
        
def is_cyclic(adj_list, titles):
    if len(titles) <= 1:
        return False
    last_title = titles[-1]
    first_title = titles[0]
    if first_title in adj_list[last_title]:
        return True
    return False

def create_adj_list(titles):
    firstWord_to_titles_hash = dict()
    result = dict()
    for title in titles:
        words = title.split()
        if len(words) != 0:
            first_word = words[0]
            if first_word not in firstWord_to_titles_hash:
                firstWord_to_titles_hash[first_word] = list()
            firstWord_to_titles_hash[first_word].append(title)
    for title in titles:
        words = title.split()
        if len(words) != 0:
            last_word = words[-1]
            if last_word in firstWord_to_titles_hash:
                titles = firstWord_to_titles_hash[last_word]
                result[title] = titles
            else:
                result[title] = list()
    return result
        
def visit_title(title, adj_list, local_visit, memo):
    if title in memo:
        return memo[title]
    result = list()
    if title not in local_visit:
        result.append(title)
        local_visit.add(title)
        neighbors = adj_list[title]
        for next_title in neighbors:
            local_result = visit_title(next_title,
                                        adj_list, 
                                        local_visit,
                                        memo)
            new_result = [title] + local_result
            if len(new_result) > len(result):
                result = new_result
        local_visit.remove(title)
    memo[title] = copy.deepcopy(result)
    return result

Input = ['OF MICE AND MEN', 'BLACK MASS', 'MEN IN BLACK']
assert(get_longest_title(Input) == 'OF MICE AND MEN IN BLACK MASS')

Input = []
assert(get_longest_title(Input) == '')

Input = ['a']
assert(get_longest_title(Input) == 'a')

Input = ['a', 'b']
assert(get_longest_title(Input) == 'a' or get_longest_title(Input) == 'b')

Input = ['a b', 'b c', 'c d', 'c e', 'e f']
assert(get_longest_title(Input) == 'a b c e f')

Input = ['e f', 'c e', 'c d','b c', 'a b']
assert(get_longest_title(Input) == 'a b c e f')

Input = ['c d','b c', 'a b', 'e f', 'c e']
assert(get_longest_title(Input) == 'a b c e f')

Input = ['a b', 'b c', 'c a']
assert(get_longest_title(Input) == 'a b c' \
       or get_longest_title(Input) == 'b c a' \
       or get_longest_title(Input) == 'c a b')

Input = ['c a', 'b c', 'a b']
assert(get_longest_title(Input) == 'a b c' \
       or get_longest_title(Input) == 'b c a' \
       or get_longest_title(Input) == 'c a b')

Input = ['b c', 'a b', 'c a']
assert(get_longest_title(Input) == 'a b c' \
       or get_longest_title(Input) == 'b c a' \
       or get_longest_title(Input) == 'c a b')
```
