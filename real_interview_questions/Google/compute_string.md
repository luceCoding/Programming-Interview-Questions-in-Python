# QUESTION
Given a string containing numbers and operators (+ and \*), calculate the result. The string can contain brackets (\[ and \]) which means you must perform that operation before adding any other operators to it.

For example, '1+2-\[3-4\]' results in 4.

# SOLUTION (Run: O(n), Space: O(n), N = number of characters in string)
This was one given during an onsite interview at google. I did this question with recursion which I believe was a mistake. 
Before the interview, he said he was testing my ability to write in Python, but the recursion method wasn't a good way to show that.
Now this solution is the dirty way to do, there is a cleaner pythonic method to this down below.

Overall, this is a medium question, as long as you can see that stacks need to be used in one way or the other, the question is fairly straight forward. By using two stacks, one to keep the numbers and one to keep the operators in, we can easily retrieve the past calculated answers and perform operations on them.

```
def calculate_string(input):
    num_stack, op_stack = list([0]), list(['+'])
    curr_index = 0
    while curr_index < len(input):
        #print(num_stack, op_stack)
        ch = input[curr_index]
        if ch.isnumeric():
            operator = op_stack.pop()
            num1 = num_stack.pop()
            num2 = int(ch)
            result = get_result(num1, num2, operator)
            num_stack.append(result)
        elif ch == '[':
            curr_index += 1
            num = int(input[curr_index])
            num_stack.append(num)
        elif ch == ']':
            num2 = num_stack.pop()
            num1 = num_stack.pop()
            operator = op_stack.pop()
            result = get_result(num1, num2, operator)
            num_stack.append(result)
        else: # operator
            op_stack.append(ch)
        curr_index += 1
    return sum(num_stack)
    
def get_result(n1, n2, operator):
    if operator == '+':
        return n1 + n2
    elif operator == '-':
        return n1 - n2

assert calculate_string('1+2-[3-4]') == 4
#assert calculate_string('1+2-[-3-4]') == 10
#assert calculate_string('11+22-[-3-44]') == 80
assert calculate_string('[1-2]+[3+4]') == 6
assert calculate_string('1+2-3-4') == -4
assert calculate_string('[1-2]+3-4') == -2
assert calculate_string('[1-2]+3-4-[5-6]') == -1
assert calculate_string('[1]-[2]+[3]-[4]') == -2
assert calculate_string('-1') == -1
assert calculate_string('-1+2') == 1
```

# SOLUTION 2 (Clean Pythonic Way)
Making use of a dictionary as a way to switch between cases, will greatly simplify the code.
'''

'''
