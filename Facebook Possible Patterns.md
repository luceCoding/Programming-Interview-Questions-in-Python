### QUESTION
Given a string as input, return the list of all the possible patterns:

```
{ "1" : ['A', 'B', 'C'],
"2" : ['D', 'E'],
"12" : ['X'],
"3" : ['P', 'Q'] }
```

Example if input is "123", then output is ["ADP","ADQ","AEP","AEQ","BDP","BDQ","BEP","BEQ","CDP","CDQ","CEP","CEQ","XP","XQ"]
Assume, you must match the input entirely in-order to produce an output.


### SOLUTION
```
def get_possible_patterns(string, dictionary, stack, start_index):
    results = list()
    if start_index >= len(string):
        return results
    end_index = start_index
    while end_index < len(string):
        pattern = string[start_index:end_index+1]
        if pattern in dictionary:
            for element in dictionary[pattern]:
                stack.append(element)
                if end_index == len(string)-1: #Are we at the end?
                    results.append(''.join(stack))
                else:
                    results += get_possible_patterns(string, 
                                                     dictionary, 
                                                     stack, 
                                                     end_index+1)
                stack.pop()
        end_index += 1
    return results

dictionary = {'1':['A','B','C'],
              '2':['D','E'],
              '12':['X'],
              '3':['P','Q']}
#dictionary = {}

string = '123'

stack = list()
results = get_possible_patterns(string, dictionary, stack, 0)
print results
```
