# QUESTION
There are an infinite number of train stops starting from station number 0.

There are an infinite number of trains. The nth train stops at all of the k * 2^(n - 1) stops where k is between 0 and infinity.

When n = 1, the first train stops at stops 0, 1, 2, 3, 4, 5, 6, etc.

When n = 2, the second train stops at stops 0, 2, 4, 6, 8, etc.

When n = 3, the third train stops at stops 0, 4, 8, 12, etc.

Given a start station number and end station number, return the minimum number of stops between them. You can use any of the trains to get from one stop to another stop. Trains can only go to a station that has a greater incresing number.

For example, the minimum number of stops between start = 1 and end = 4 is 3 because we can get from 1 to 2 to 4.

# EXPLAINATION
The idea is to create a dynamic programming 2d array. The columns represent the stations and the rows are the trains. If I wanted to go from station 7 to station 14. I would need to know the min stops for station 7,8,9,10,11,12,13, and 14. While using trains that start with the values 1,2,4,8, essentially trains n=1 to n=4. Notice I don't care about n=5 or 16 because its above the given end.

I use 2^row_index to find the starting train value, like, 1,2,4,8,16,32, basically one bit shifted over to the right. With this I can mod to the current station to see if the train stops at this station. If it does, I need to find the previous station that the train stopped at, which would be col_index - curr_train. Using that, each cell in the array will be the min n_stops for that station. The cell above it will represent the min n_stops without the current train and the cells to the left of it will represent min n_stops WITH the current train.

# SOLUTION
```
def find_n_stops(start, end):
    if end < start:
        return 0
    if start == end:
        return 1
    n_cols = end - start + 1
    n_rows = 1
    count = 1
    # need to figure out largest 2^signficiant bit to represent the rows
    # since trains go from 1,2,4,8,16... 
    # we don't need all the trains, just the ones equal or less than the given end
    while count <= end:
        count <<= 1
        if count <= end:
            n_rows += 1
    # rows are the trains and the cols are the stations
    dp = [[0 for _ in range(n_cols)] for _ in range(n_rows)]
    # intalize first row to 0,1,2,3,4...etc...
    # first row represents train 1
    for col_index in range(0, len(dp[0])):
        dp[0][col_index] = col_index
    for row_index in range(1, len(dp)):
        for col_index in range(1, len(dp[0])):
            curr_station = col_index + start
            curr_train = 2 ** row_index
            if curr_train > curr_station: # we can't use this train
                dp[row_index][col_index] = min(dp[row_index-1][col_index],
                                               dp[row_index][col_index-1])+1
            else:
                if (curr_station % curr_train) == 0: # we can use this train
                    # this train stops at this station
                    prev_station_col_index = col_index - curr_train
                    if prev_station_col_index >= 0:
                        dp[row_index][col_index] = min(dp[row_index-1][col_index],
                                                       dp[row_index][prev_station_col_index]+1)
                    else:
                        dp[row_index][col_index] = min(dp[row_index-1][col_index],
                                                       dp[row_index][col_index-1]+1)
                else: # we can't use this train
                    dp[row_index][col_index] = min(dp[row_index-1][col_index],
                                                   dp[row_index][col_index-1]+1)
    return dp[-1][-1]+1
print find_n_stops(7,14)
```

# FOLLOW UP QUESTION
What happens if trains were able to go in both directions? 

For example, the minimum number of stops between start = 1 and end = 8 is 3 because we can get from 1 to 0 to 8.
