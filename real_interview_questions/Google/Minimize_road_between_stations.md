# QUESTION
There is long motorway of length M (an integer). 
On the motorway at some points (integers) there are petrol stations (there is a given integer array T[] of length N - the positions of petrol stations). 
We have funds to add at most K new petrol stations on this motorway. 
We want to minimize largest stretch of the motorway between petrol stations.
You can put petrol stations on integer positions.

1. Design and implement a function that calculates the minimum achievable uncovered motorway length.
2. Write testcases for your algorithm.
3. (Bonus) Analyze your algorithm's runtime and space complexity.
4. (Bonus) Suggest optimizations of your algorithm.
5. (Bonus) Modify the function from part 1 to output positions at which the new stations can be added to
   achieve the optimum uncovered motorwary length.

Example: 
N = 5, M = 20, K = 3, T = [3, 7, 15, 18, 1] 
Minimum uncovered : 3 (obtainable for example by adding petrol stations at positions: 5, 10 and 13) 

St-Ps----Ps----Np----Ps-------Np-------Np----Ps-------Ps----Fn 
00 01 02 03 04 05 06 07 08 09 10 11 12 13 14 15 16 17 18 19 20 

St - start of motorway
Fn - finish of motorway 
Ps - pre-existing petrol station 
Np - new petrol station in optimal solution

# SOLUTION
```
import heapq

def place_stations(n_new_stations, length_of_road, stations):
    heap = create_max_heap_with(stations, length_of_road)
    n_stations_placed = 0
    while len(heap) != 0 and n_new_stations > 0:
        longest_road = heapq.heappop(heap)
        print longest_road
        new_road = place_station_on_road(longest_road)
        if new_road[0] != 0: # check distance between next station
            heapq.heappush(heap, new_road)
        n_new_stations -= 1
        n_stations_placed += 1
    return n_stations_placed
    
def create_max_heap_with(stations, length_of_road):
    heap = list()
    sorted_station = stations.sort()
    distances = list()
    start_index = 0
    for element in stations:
        len_of_road = element - start_index
        if len_of_road != 0:
            distances.append(len_of_road)
        start_index = element + 1
    if length_of_road != start_index: # get last distance
        len_of_road = length_of_road - start_index
        distances.append(len_of_road)
    for len_of_road in distances:
        # distances between stations, n_stations_on_road, total length of road
        node = (-len_of_road, 0, len_of_road) 
        heapq.heappush(heap, node)
    return heap
    
def place_station_on_road(road):
    distance_between = road[0]
    n_stations_on_road = road[1]
    length_of_road = road[2]
    n_stations_on_road += 1
    if n_stations_on_road >= length_of_road:
        return (0, n_stations_on_road, length_of_road)
    distance_between = float(n_stations_on_road) / float(length_of_road)
    node = (-distance_between, n_stations_on_road, length_of_road)
    return node
    
M = 5
K = 3
T = [0,1,2,3,4]

print place_stations(K, M, T)
```
