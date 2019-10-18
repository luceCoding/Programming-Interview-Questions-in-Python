# Always return something, don't modify
Ideal, we want to write functions that always return something back. 
It gets confusing when you have a list of functions and there isn't a clear distinction on whether if passing an object into that function modifies your object or not.
So that is why its generally a good rule of thumb to instead return an object versus passing an object to modify it.

There are some exception during interviews, it tends to deal with recursion. 
Sometimes there can be a problem where you are storing a temporary result and until reaching a base case like the end of a list, do you save your temporary result into the final result.
In this scenario, you will have to pass in a mutable for each recursion because you won't know when the save trigger occurs.

However, outside of recursion, I would recommend to always return an object instead of passing in an object and modifying it.
There are some times exceptions to this depending on the question.

# SOLID Principles
### Single Responsiblity Principle
### Open/Closed Principle
### Liskov Substitution Principle
### Interface Segregation Principle
### Dependency Inversion

# Python Style Guide
https://google.github.io/styleguide/pyguide.html
