# QUESTION
Given a compressed string in which a number followed by [] indicate how many times those characters occur, decompress the string

Eg. : a3[b2[c1[d]]]e will be decompressed as abcdcdbcdcdbcdcde.

Assume the string is well formed and number will always be followed by a [].

# EXPLAINATION
Yes, this question can get very complicated. Notice that 'a' is in the front of 'bcdcdbcdcdbcdcd' which corresponds to '[b2[c1[d]]]'. Then 'e' is at the end. You can think of 'a' and 'e' as a start of a new set of decompression/calculations.

The idea is to use recursion to decompress the string. My solution does it iteratively instead.

First break up the string into tokens, 'a3[b2[c1[d]]]e' -> 'a3','[','b2','[','c','[','d',']',']',']','e'

We will use two stacks, one stack(result stack) stores results of the decompressed string, which will be joined at the end. 
And another stack(token stack) keeps track of what tokens need to be decompressed. 
When encountering an open bracket, push the next token into the token stack. 
If we encounter a closed bracket, we should grab the top token then duplicate what was on the top result stack X times then append that token to the front and save it into the result stack. 
If we instead encounter a token, the suggests that a new result needs to be calculated, this is what was observed with 'a' and 'e' from above. That means any tokens remaining need to be calculated and saved into the result stack before we can start anew. A small trick is to place an empty string into the result stack as a placeholder for calculating the next set of decompression.

# SOLUTION
Need to clarify what 'a2[b2[c]d2[e]]f' would decompress to. I would assume to 'abccdeebccdeef'.
```
import re

def decompress(compressed_string):
    tokens = re.split('([\[|\]])', compressed_string)
    tokens = [token for token in tokens if token != ''] # remove extra spaces
    if len(tokens) == 0:
        return ''
    token_stack = list()
    result_stack = list()
    curr_token_index = 0
    while curr_token_index < len(tokens):
        curr_token = tokens[curr_token_index]
        if curr_token == '[':
            curr_token_index += 1
            next_token = tokens[curr_token_index]
            token_stack.append(next_token)
        elif curr_token == ']':
            next_result = get_next_result(token_stack, result_stack)
            result_stack.append(next_result)
        else:
            while len(token_stack):
                next_result = get_next_result(token_stack, result_stack)
                result_stack.append(next_result)
            token_stack.append(curr_token)
            result_stack.append('')
        curr_token_index += 1
    while len(token_stack):
        next_result = get_next_result(token_stack, result_stack)
        result_stack.append(next_result)
    return ''.join(result_stack)    

def get_next_result(token_stack, result_stack):
    if len(token_stack):
        token = token_stack.pop()
        top_result = ''
        if len(result_stack):
            top_result = result_stack.pop()
        word, n_dups = parse_token(token)
        new_result = ''
        for _ in range(n_dups):
            new_result += top_result
        new_result = word + new_result
        return new_result
    return ''

def parse_token(token):
    last_alpha_index = -1
    for letter in token:
        if not letter.isdigit():
            last_alpha_index += 1
    word = token[0:last_alpha_index+1]
    number = 1
    if last_alpha_index != len(token)-1:
        number = int(token[last_alpha_index+1:])
    return word, number

string = 'a3[b2[c1[d]]]e'
assert decompress(string) == 'abcdcdbcdcdbcdcde'

# string = 'a2[b2[c]d2[e]]f'
# assert decompress(string) == 'abccdeebccdeef'
```
