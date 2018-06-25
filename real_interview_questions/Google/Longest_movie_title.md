# SOLUTION
```
import copy

def get_longest_title(titles):
    adj_list = create_adj_list(titles)
    global_visit = set()
    longest_title = list()
    memo = dict()
    for title in adj_list.keys():
        if title not in global_visit:
            local_visit = set()
            local_title = list()
            local_result = visit_title(title, 
                                        local_title,
                                        adj_list, 
                                        local_visit, 
                                        global_visit,
                                        memo)
            if len(local_result) > len(longest_title):
                longest_title = copy.deepcopy(local_result)
    return concat_title(longest_title)

def concat_title(titles):
    if len(titles) == 0:
        return ''
    result = ''
    result = titles[0]
    for index in range(1, len(titles)):
        title = titles[index]
        sub_title = title[1:]
        result += sub_title
    return result
        

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
        
def visit_title(title, stack, adj_list, local_visit, global_visit, memo):
    if title in memo:
        return memo[title]
    result = list()
    if title not in local_visit:
        local_visit.add(title)
        stack.append(title)
        neighbors = adj_list[title]
        for next_title in neighbors:
            local_result = visit_title(next_title, 
                                        stack,
                                        adj_list, 
                                        local_visit, 
                                        global_visit,
                                        memo)
            if len(local_result) > len(result):
                result = copy.deepcopy(local_result)
        if len(stack) > len(result):
                result = copy.deepcopy(stack)
        stack.pop()
        local_visit.remove(title)
    memo[title] = result 
    return result
        
Input = ['OF MICE AND MEN', 'BLACK MASS', 'MEN IN BLACK']
#Input = ['a b', 'b c', 'c d', 'c e', 'e f']  
#Input = ['a b', 'b c', 'c a'] 
Input = ['e f', 'c e', 'c d','b c', 'a b']
#Input = ['c d','b c', 'a b', 'e f', 'c e']
    
print get_longest_title(Input)
```
