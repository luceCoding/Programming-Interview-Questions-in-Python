```
import heapq

def place_stations(n_new_stations, length_of_road, stations):
    heap = create_heap_with(stations, length_of_road)
    n_stations_placed = 0
    while len(heap) != 0 and n_new_stations != n_stations_placed:
        longest_road = heapq.heappop(heap)
        new_road = place_station_on_road(longest_road)
        if new_road[0] != 0: # check distance between next station
            heapq.heappush(heap, new_road)
        n_new_stations -= 1
        n_stations_placed += 1
    return n_stations_placed
    
def create_heap_with(stations, length_of_road):
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
        node = (len_of_road, 0, len_of_road) 
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
    node = (distance_between, n_stations_on_road, length_of_road)
    return node
    
    
M = 5
K = 3
T = [0,1,2,4]

print place_stations(K, M, T)
```
