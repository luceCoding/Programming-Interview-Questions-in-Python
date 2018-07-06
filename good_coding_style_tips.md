# Always return something, don't modify
Ideal, we want to write functions that always return something back. 
It gets confusing when you have a list of functions and there isn't a clear distinction on whether if passing an object into that function modifies your object or not.
So that is why its generally a good rule of thumb to instead return an object versus passing an object to modify it.

There are some exception during interviews, it tends to deal with recursion. Sometimes there can be a problem where you are storing a temporary result and until some thing like reaching the end of the index, do you save it into the global result.
In this scenario, you will have to pass in a global_result object list for each recursion because you won't know when the save trigger occurs.

However, outside of recursion, I would recommend to always return an object over passing in an object and modifying it.
